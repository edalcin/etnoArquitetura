# ADR-001: Abordagem de Armazenamento de Dados

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
1. Armazenar dados estruturados e semi-estruturados (JSON)
2. Suportar consultas relacionais
3. Indexação eficiente para buscas
4. Transações ACID para operações críticas
5. Capacidade de sincronização e replicação de dados

### Não-Funcionais
1. Escalabilidade horizontal
2. Performance: queries < 100ms (p95)
3. Alta disponibilidade (99.5% uptime)
4. Backup e recuperação automatizados
5. Comunidade ativa e suporte

## Abordagens Consideradas

### Abordagem 1: Bancos de Dados SQL (Relacionais)

**Prós:**
- Maturidade e estabilidade comprovadas
- Transações ACID completas
- Integridade referencial garantida
- Queries poderosas em dados estruturados
- Suporte a dados geoespaciais (extensões)
- Excelente para dados fortemente relacionados

**Contras:**
- Schema relativamente rígido (requer migração para mudanças estruturais)
- Joins complexos podem ser custosos em dados M:N
- Menos natural para dados fortemente aninhados
- Escalabilidade horizontal requer ferramentas adicionais

**Caso de Uso Ideal:** Dados altamente estruturados com relações bem definidas

### Abordagem 2: Bancos de Dados Orientados a Documentos (JSON)

**Prós:**
- Schema flexível por padrão (schemaless)
- Excelente para documentos aninhados e complexos
- Escalabilidade horizontal nativa
- Replicação e sincronização em tempo real
- Pipeline de agregação poderoso para análises
- Natural para dados semi-estruturados

**Contras:**
- Joins entre documentos menos eficientes que SQL
- Transações complexas têm overhead
- Requer planejamento cuidadoso de índices
- Pode levar a duplicação de dados (desnormalização)

**Caso de Uso Ideal:** Documentos complexos e hierárquicos com schema evolutivo

### Abordagem 3: Bancos de Dados Multi-Modais

**Prós:**
- Suportam múltiplos modelos de dados (documentos, grafos, relações)
- Modelam relações complexas tipo grafo de forma eficiente
- Flexibilidade de schema com suporte a múltiplos paradigmas
- Subscriptions em tempo real nativas
- Simplificam relacionamentos complexos

**Contras:**
- Tecnologia relativamente nova (maturidade variável)
- Comunidades podem ser menores
- Casos de uso em produção variam
- Documentação em desenvolvimento contínuo

**Caso de Uso Ideal:** Dados com relações complexas tipo grafo, taxonomia e necessidade de flexibilidade

## Decisão

**Recomendamos uma arquitetura orientada a documentos (JSON) como solução principal, com avaliação de abordagens multi-modais para casos específicos.**

### Justificativa

#### Por que Orientado a Documentos (JSON)?

1. **Flexibilidade de Schema**: Conhecimento tradicional é intrinsecamente heterogêneo. Modelo JSON permite:
   ```json
   {
     "type": "medicinal_plant",
     "scientificName": "Manihot esculenta",
     "uses": [{
       "description": "Tratamento de feridas",
       "preparationMethod": "Cataplasma de folhas",
       "dosage": "Aplicar 2x ao dia"
     }]
   }

   {
     "type": "ritual",
     "name": "Benzimento",
     "relatedSpecies": ["..."],
     "steps": ["...", "..."],
     "seasonality": "Lua cheia"
   }
   ```

2. **Análises Complexas**: Pipeline de agregação permite análises avançadas em documentos
3. **Sincronização**: Capacidade nativa de sincronização e replicação
4. **Escalabilidade**: Escalamento horizontal para crescimento
5. **Maturidade**: Ecossistema robusto e amplamente adotado

#### Por que não SQL (exclusivamente)?

Embora bancos SQL sejam extremamente robustos, a natureza dos dados favorece um modelo orientado a documentos:

- Relações M:N complexas (espécie ↔ usos ↔ comunidades ↔ regiões) implicam múltiplas tabelas de junção
- Evolução frequente de schema seria custosa em SQL
- Dados aninhados naturais em JSON são incômodos em modelo relacional

#### Por que não Multi-Modal (ainda)?

Bancos multi-modais são promissores para modelar relações tipo grafo (taxonomia, relações entre comunidades). No entanto:

- **Maturidade**: Tecnologias variam em nível de consolidação
- **Produção**: Casos de uso em larga escala podem ser limitados
- **Risco**: Menos histórico de deployments em larga escala

**Recomendação**: Avaliar abordagens multi-modais em fase futura (v2.0 do sistema) quando integração com sistemas de grafo for crítica.

## Estrutura de Dados em Formato JSON

### Entidades Principais

#### 1. Registros de Conhecimento

```json
{
  "id": "rec_123456",
  "type": "traditional_knowledge",
  "status": "published",
  "version": 3,

  "species": {
    "scientificName": "Manihot esculenta Crantz",
    "commonNames": ["Mandioca", "Cassava", "Aipim"],
    "family": "Euphorbiaceae",
    "kingdom": "Plantae",
    "taxonKey": "5290063",
    "verified": true
  },

  "uses": [
    {
      "category": "medicinal",
      "description": "Tratamento de inflamações",
      "preparationMethod": "Chá das folhas",
      "dosage": "200ml 3x ao dia",
      "partUsed": "folhas",
      "administration": "oral",
      "ailments": ["inflamação", "dor"]
    },
    {
      "category": "food",
      "description": "Farinha para alimentos",
      "preparationMethod": "Ralado e torrado",
      "partUsed": "raiz"
    }
  ],

  "location": {
    "region": "Amazônia",
    "country": "Brasil",
    "state": "Amazonas",
    "coordinates": {
      "type": "Point",
      "latitude": -3.1190,
      "longitude": -60.0217
    }
  },

  "community": {
    "name": "Comunidade Indígena XYZ",
    "ethnicity": "Tikuna"
  },

  "source": {
    "type": "secondary",
    "reference": "Silva et al. (2023)",
    "doi": "10.1234/example",
    "year": 2023,
    "collectionDate": "2023-05-15"
  },

  "metadata": {
    "createdAt": "2025-01-15T10:30:00Z",
    "createdBy": "user_123",
    "lastModifiedAt": "2025-01-20T14:20:00Z",
    "lastModifiedBy": "curator_456",
    "publishedAt": "2025-01-21T09:00:00Z"
  },

  "permissions": {
    "visibility": "public",
    "allowedUsers": [],
    "allowedCommunities": []
  },

  "images": [
    {
      "filename": "plant_123_001.jpg",
      "caption": "Folhas da planta",
      "credit": "Fotógrafo XYZ",
      "license": "CC BY-SA 4.0"
    }
  ],

  "tags": ["etnobotânica", "medicina tradicional", "Amazônia"]
}
```

#### 2. Usuários

```json
{
  "id": "user_123",
  "email": "researcher@example.com",
  "name": "Dr. João Silva",
  "role": "curator",
  "community": "community_456",
  "institution": "Universidade Federal do Amazonas",
  "permissions": ["read", "write", "approve"],
  "createdAt": "2024-01-15T10:30:00Z",
  "lastLogin": "2025-01-22T14:20:00Z"
}
```

#### 3. Logs de Auditoria

```json
{
  "id": "audit_789",
  "recordId": "rec_123456",
  "versionNumber": 2,
  "action": "update",
  "userId": "user_123",
  "timestamp": "2025-01-20T14:20:00Z",
  "changes": {
    "field": "species.scientificName",
    "oldValue": "Manihot esculenta",
    "newValue": "Manihot esculenta Crantz"
  },
  "comments": "Corrigido nome científico após validação GBIF"
}
```

### Índices Recomendados

Os seguintes campos devem ser indexados para otimizar performance:

- `species.scientificName` - para buscas por espécie
- `species.family` - para buscas por família
- `uses.category` - para filtros de uso
- `location.coordinates` - para buscas geoespaciais
- `status`, `createdAt` - para ordenação e filtro
- `tags` - para busca por tag
- `source.doi` - para buscas por DOI
- Busca textual em múltiplos campos (nome científico, nomes comuns, descrições)

## Estratégia de Evolução Futura

Para crescimento futuro:

1. **Fase 1 (Atual)**: Orientado a documentos (JSON) como sistema principal
2. **Fase 2**: Avaliação de multi-modelos para taxonomia (6-12 meses)
3. **Fase 3**: Integração de grafo para relações complexas se necessário
   - Manter orientado a documentos como fonte de verdade
   - Grafo como índice secundário para consultas específicas
   - Sincronização bidirecional entre os modelos

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
