# ADR-001: Seleção de Banco de Dados

## Status
**Proposto** - Aguardando validação técnica

## Contexto

O Sistema de Conhecimento Tradicional precisa armazenar dados altamente complexos e heterogêneos:

- **Estrutura Variável**: Diferentes tipos de conhecimento tradicional possuem atributos distintos (plantas medicinais vs. rituais vs. construção)
- **Relações Complexas**: Espécies relacionadas a múltiplos usos, comunidades, regiões e fontes
- **Evolução do Schema**: Novos tipos de conhecimento podem surgir, exigindo flexibilidade
- **Volume**: Estimativa de centenas de milhares de registros a longo prazo
- **Consultas Complexas**: Necessidade de atravessar relações (ex: "espécies usadas por comunidade X na região Y")
- **Versionamento**: Rastreamento de alterações históricas nos registros

## Requisitos

### Funcionais
1. Armazenar documentos JSON com estrutura flexível
2. Suportar consultas relacionais (joins)
3. Indexação eficiente para buscas
4. Transações ACID para operações críticas
5. Change streams para sincronização em tempo real

### Não-Funcionais
1. Escalabilidade horizontal
2. Performance: queries < 100ms (p95)
3. Alta disponibilidade (99.5% uptime)
4. Backup e recuperação automatizados
5. Comunidade ativa e suporte

## Opções Consideradas

### Opção 1: PostgreSQL + JSONB

**Prós:**
- Maturidade e estabilidade comprovadas
- Suporte robusto a JSON (JSONB) com indexação
- Transações ACID completas
- Extensões poderosas (PostGIS para dados geográficos)
- Queries relacionais tradicionais

**Contras:**
- Schema ainda relativamente rígido
- Joins complexos em JSONB podem ser lentos
- Escalabilidade horizontal requer ferramentas externas (Citus)
- Menos natural para dados fortemente aninhados

**Caso de Uso Ideal:** Dados estruturados com alguma flexibilidade JSON

### Opção 2: MongoDB

**Prós:**
- Schema flexível por padrão (schemaless)
- Excelente para documentos aninhados e complexos
- Escalabilidade horizontal nativa (sharding)
- Change streams para eventos em tempo real
- Aggregation pipeline poderoso
- Grande comunidade e ecossistema

**Contras:**
- Joins (lookups) menos eficientes que SQL
- Transações multi-documento têm overhead
- Requer planejamento cuidadoso de índices
- Pode levar a duplicação de dados (desnormalização)

**Caso de Uso Ideal:** Documentos complexos e hierárquicos com schema evolutivo

### Opção 3: SurrealDB

**Prós:**
- Multi-modelo: documentos + grafos + relações
- Suporta relações nativas (como SQL) e grafos
- Query language moderna (SurrealQL)
- Flexibilidade de schema com tipagem opcional
- Real-time subscriptions nativas
- Embedding de relações simplificado

**Contras:**
- Tecnologia relativamente nova (menos madura)
- Comunidade menor
- Menos ferramentas de terceiros
- Casos de uso em produção limitados
- Documentação ainda em desenvolvimento

**Caso de Uso Ideal:** Dados com relações complexas tipo grafo e necessidade de flexibilidade

## Decisão

**Recomendamos MongoDB como solução principal, com avaliação futura de SurrealDB.**

### Justificativa

#### Por que MongoDB?

1. **Flexibilidade de Schema**: Conhecimento tradicional é intrinsecamente heterogêneo. MongoDB permite:
   ```javascript
   // Registro de planta medicinal
   {
     type: "medicinal_plant",
     scientificName: "Manihot esculenta",
     uses: [{
       description: "Tratamento de feridas",
       preparationMethod: "Cataplasma de folhas",
       dosage: "Aplicar 2x ao dia"
     }]
   }

   // Registro de ritual
   {
     type: "ritual",
     name: "Benzimento",
     plants: [ObjectId("...")],
     steps: ["...", "..."],
     seasonality: "Lua cheia"
   }
   ```

2. **Aggregation Pipeline**: Permite análises complexas:
   ```javascript
   db.records.aggregate([
     { $match: { "uses.category": "medicinal" } },
     { $unwind: "$uses" },
     { $group: { _id: "$family", count: { $sum: 1 } } },
     { $sort: { count: -1 } }
   ])
   ```

3. **Change Streams**: Sincronização em tempo real com Elasticsearch:
   ```javascript
   const changeStream = collection.watch();
   changeStream.on('change', (change) => {
     // Atualizar índice de busca
     elasticsearch.index(change.fullDocument);
   });
   ```

4. **Escalabilidade**: Sharding nativo para crescimento horizontal
5. **Ecossistema**: Ferramentas maduras (Compass, Atlas, mongoose ODM)

#### Por que não PostgreSQL?

Embora PostgreSQL seja extremamente robusto, a natureza dos dados favorece um modelo orientado a documentos:

- Relações M:N complexas (espécie ↔ usos ↔ comunidades ↔ regiões) seriam múltiplas tabelas de junção
- JSONB é poderoso, mas ainda requer schema de tabela principal
- Escalabilidade horizontal mais complexa

#### Por que não SurrealDB (ainda)?

SurrealDB é promissor, especialmente para modelar relações tipo grafo (ex: taxonomia, relações entre comunidades). No entanto:

- **Risco de Maturidade**: Projeto lançado em 2022, ainda em rápida evolução
- **Produção**: Poucos casos de uso em larga escala documentados
- **Equipe**: Risco de lock-in em tecnologia emergente

**Recomendação**: Avaliar SurrealDB em fase futura (v2.0 do sistema) quando a tecnologia estiver mais madura.

## Estrutura de Dados no MongoDB

### Coleções Principais

#### 1. `records` (Registros de Conhecimento)
```javascript
{
  _id: ObjectId("..."),
  type: "traditional_knowledge",
  status: "published", // pending, in_review, approved, published, rejected
  version: 3,

  // Dados Taxonômicos
  species: {
    scientificName: "Manihot esculenta Crantz",
    commonNames: ["Mandioca", "Cassava", "Aipim"],
    family: "Euphorbiaceae",
    kingdom: "Plantae",
    taxonKey: 5290063, // GBIF key
    verified: true
  },

  // Usos Tradicionais
  uses: [
    {
      category: "medicinal",
      description: "Tratamento de inflamações",
      preparationMethod: "Chá das folhas",
      dosage: "200ml 3x ao dia",
      partUsed: "folhas",
      administration: "oral",
      ailments: ["inflamação", "dor"]
    },
    {
      category: "food",
      description: "Farinha para alimentos",
      preparationMethod: "Ralado e torrado",
      partUsed: "raiz"
    }
  ],

  // Localização
  location: {
    region: "Amazônia",
    country: "Brasil",
    state: "Amazonas",
    coordinates: {
      type: "Point",
      coordinates: [-60.0217, -3.1190] // [longitude, latitude]
    }
  },

  // Comunidade (dados públicos apenas)
  community: {
    name: "Comunidade Indígena XYZ", // Opcional, requer consentimento
    ethnicity: "Tikuna"
  },

  // Fonte
  source: {
    type: "secondary", // primary | secondary
    reference: "Silva et al. (2023)",
    doi: "10.1234/example",
    url: "https://...",
    year: 2023,
    collectedBy: ObjectId("user_id"), // Para fontes primárias
    collectionDate: ISODate("2023-05-15")
  },

  // Metadados
  createdAt: ISODate("2025-01-15T10:30:00Z"),
  createdBy: ObjectId("user_id"),
  lastModifiedAt: ISODate("2025-01-20T14:20:00Z"),
  lastModifiedBy: ObjectId("curator_id"),
  publishedAt: ISODate("2025-01-21T09:00:00Z"),

  // Controle de Acesso
  permissions: {
    visibility: "public", // public | restricted | private
    allowedUsers: [], // Para registros restritos
    allowedCommunities: []
  },

  // Imagens
  images: [
    {
      filename: "plant_123_001.jpg",
      caption: "Folhas da planta",
      credit: "Fotógrafo XYZ",
      license: "CC BY-SA 4.0"
    }
  ],

  // Tags
  tags: ["etnobotânica", "medicina tradicional", "Amazônia"]
}
```

#### 2. `users` (Usuários)
```javascript
{
  _id: ObjectId("..."),
  email: "researcher@example.com",
  name: "Dr. João Silva",
  role: "curator", // researcher | curator | admin | community_rep
  community: ObjectId("community_id"), // Para community_rep
  institution: "Universidade Federal do Amazonas",
  permissions: ["read", "write", "approve"],
  createdAt: ISODate("..."),
  lastLogin: ISODate("...")
}
```

#### 3. `audit_logs` (Auditoria e Versionamento)
```javascript
{
  _id: ObjectId("..."),
  recordId: ObjectId("record_id"),
  versionNumber: 2,
  action: "update", // create | update | approve | reject | publish
  userId: ObjectId("user_id"),
  timestamp: ISODate("..."),
  changes: {
    field: "species.scientificName",
    oldValue: "Manihot esculenta",
    newValue: "Manihot esculenta Crantz"
  },
  snapshot: { /* full document snapshot */ },
  comments: "Corrigido nome científico após validação GBIF"
}
```

### Índices

```javascript
// records collection
db.records.createIndex({ "species.scientificName": 1 });
db.records.createIndex({ "species.family": 1 });
db.records.createIndex({ "uses.category": 1 });
db.records.createIndex({ "location.coordinates": "2dsphere" }); // Geoespacial
db.records.createIndex({ status: 1, createdAt: -1 });
db.records.createIndex({ tags: 1 });
db.records.createIndex({ "source.doi": 1 }, { unique: true, sparse: true });

// Text search
db.records.createIndex({
  "species.scientificName": "text",
  "species.commonNames": "text",
  "uses.description": "text"
}, { weights: { "species.scientificName": 10, "species.commonNames": 5 } });

// users collection
db.users.createIndex({ email: 1 }, { unique: true });
db.users.createIndex({ role: 1 });

// audit_logs collection
db.audit_logs.createIndex({ recordId: 1, versionNumber: -1 });
db.audit_logs.createIndex({ timestamp: -1 });
```

## Estratégia de Migração Futura (SurrealDB)

Se decidirmos migrar para SurrealDB no futuro:

1. **Fase 1 (Atual)**: MongoDB como sistema principal
2. **Fase 2**: Avaliação piloto de SurrealDB para subset de dados (6 meses)
3. **Fase 3**: Migração gradual se piloto for bem-sucedido
   - Manter MongoDB como read replica durante transição
   - Sincronização bidirecional temporária
   - Migração por partes (por região, por tipo de dado)

## Consequências

### Positivas
- Flexibilidade para evoluir modelo de dados sem migrações complexas
- Performance adequada para volume esperado
- Ferramentas de monitoramento e backup maduras
- Facilita onboarding de desenvolvedores (MongoDB é amplamente conhecido)

### Negativas
- Joins menos eficientes que SQL (mitigado com desnormalização estratégica)
- Risco de inconsistência em dados duplicados (requer atenção em design)
- Custos de hosting podem ser maiores que PostgreSQL para mesmo volume

### Neutras
- Necessidade de Elasticsearch para busca avançada (seria necessário de qualquer forma)
- Requer disciplina em modelagem de dados (evitar anti-patterns)

## Métricas de Sucesso

Avaliar a decisão após 6 meses:

1. **Performance**: 95% das queries < 100ms
2. **Escalabilidade**: Suporta crescimento de 10x sem re-arquitetura
3. **Facilidade de Uso**: Time consegue iterar rapidamente em mudanças de schema
4. **Custos**: Dentro do orçamento projetado

## Referências

- [MongoDB Best Practices for Schema Design](https://www.mongodb.com/developer/products/mongodb/schema-design-anti-pattern-summary/)
- [GBIF Data Model](https://www.gbif.org/data-model)
- [Darwin Core Standard](https://dwc.tdwg.org/)
- [SurrealDB Documentation](https://surrealdb.com/docs)

## Data de Revisão

**Próxima Revisão**: 6 meses após implementação inicial (estimado: Agosto 2025)
