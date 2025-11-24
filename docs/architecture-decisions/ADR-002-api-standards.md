# ADR-002: Padrões de API e Integração

## Status
**Proposto** - Aguardando validação técnica

## Contexto

O sistema possui três contextos com necessidades distintas de API:

1. **Aquisição**: Ingestão de dados de múltiplas fontes (pesquisadores, robôs, sistemas externos)
2. **Curadoria**: Interface interna para workflow de validação
3. **Apresentação**: API pública para consultas complexas

Precisamos definir:
- Estilo arquitetural (REST vs GraphQL vs gRPC)
- Padrões de autenticação e autorização
- Formato de payload e versionamento
- Tratamento de erros
- Documentação

## Requisitos

### Funcionais
1. Suportar ingestão de dados estruturados (JSON)
2. Permitir consultas complexas com filtros e relacionamentos
3. Versionamento de API para retrocompatibilidade
4. Documentação interativa e auto-gerada

### Não-Funcionais
1. Desempenho: < 200ms para 95% das requisições
2. Segurança: Autenticação robusta, rate limiting
3. Escalabilidade: Suportar 1000+ req/min
4. Developer Experience: APIs intuitivas e bem documentadas

## Decisão

**Abordagem Híbrida:**
- **REST** para contextos de Aquisição e Curadoria
- **GraphQL** para contexto de Apresentação (API Pública)

### Justificativa

#### REST para Aquisição e Curadoria

**Por quê?**
1. **Operações CRUD Simples**: Criar, atualizar, aprovar registros
2. **Cacheable**: Métodos GET podem ser cached facilmente
3. **Maturidade**: Ferramentas de debugging e monitoring abundantes
4. **Simplicidade**: Mais fácil para pesquisadores integrarem

**Exemplo:**
```http
POST /api/acquisition/records
PUT /api/curation/records/:id
POST /api/curation/records/:id/approve
```

#### GraphQL para Apresentação

**Por quê?**
1. **Consultas Complexas**: Usuários públicos precisam de dados relacionados
   ```graphql
   query {
     species(id: "123") {
       scientificName
       uses {
         description
         community
       }
       regions {
         name
         country
       }
     }
   }
   ```

2. **Flexibilidade**: Clientes podem requisitar exatamente o que precisam (evita over-fetching)
3. **Single Endpoint**: Simplifica roteamento e caching
4. **Type System**: Auto-documentação via schema
5. **Real-time**: Subscriptions para atualizações (futuro)

**Por que NÃO gRPC?**
- Foco em integrações web/mobile (JSON é mais universal)
- Clientes públicos diversos (gRPC requer bibliotecas específicas)
- Ferramentas de inspeção (Postman, curl) menos amigáveis com gRPC

---

## Padrões REST (Aquisição e Curadoria)

### Estrutura de URLs

```
Base URL: https://api.etnoknowledge.org/v1

Contexto Aquisição:
POST   /acquisition/records              - Criar registro
POST   /acquisition/records/bulk         - Criar múltiplos
GET    /acquisition/records/:id          - Obter registro
GET    /acquisition/records/:id/status   - Status de processamento
POST   /acquisition/validate             - Validar sem criar

Contexto Curadoria:
GET    /curation/records                 - Listar (com filtros)
GET    /curation/records/:id             - Obter detalhes
PUT    /curation/records/:id             - Atualizar
PATCH  /curation/records/:id             - Atualização parcial
POST   /curation/records/:id/approve     - Aprovar
POST   /curation/records/:id/reject      - Rejeitar
GET    /curation/records/:id/history     - Histórico de versões
POST   /curation/records/:id/assign      - Atribuir curador
```

### Convenções de Nomenclatura

- **Recursos**: substantivos plurais (`/records`, `/users`)
- **Ações**: verbos HTTP padrão (GET, POST, PUT, PATCH, DELETE)
- **Sub-recursos**: `/resources/:id/sub-resources`
- **Ações especiais**: POST em sub-recurso (`/records/:id/approve`)

### Request Payload

```json
// POST /acquisition/records
{
  "species": {
    "scientificName": "Manihot esculenta",
    "commonNames": ["Mandioca", "Cassava"],
    "family": "Euphorbiaceae"
  },
  "uses": [
    {
      "category": "medicinal",
      "description": "Tratamento de feridas",
      "preparationMethod": "Cataplasma das folhas"
    }
  ],
  "location": {
    "region": "Amazônia",
    "country": "Brasil",
    "coordinates": {
      "latitude": -3.1190,
      "longitude": -60.0217
    }
  },
  "source": {
    "type": "primary",
    "reference": "Coleta em campo, 2025",
    "collectionDate": "2025-01-15"
  }
}
```

### Response Payload

**Sucesso (201 Created):**
```json
{
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "status": "pending",
    "createdAt": "2025-01-15T10:30:00Z",
    "species": {
      "scientificName": "Manihot esculenta",
      "commonNames": ["Mandioca", "Cassava"]
    }
  },
  "meta": {
    "requestId": "req_abc123",
    "processingTime": 45
  }
}
```

**Erro (400 Bad Request):**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Falha na validação dos dados",
    "details": [
      {
        "field": "species.scientificName",
        "message": "Nome científico é obrigatório",
        "code": "REQUIRED_FIELD"
      },
      {
        "field": "uses[0].category",
        "message": "Categoria inválida. Valores aceitos: medicinal, food, ritual, construction",
        "code": "INVALID_ENUM"
      }
    ]
  },
  "meta": {
    "requestId": "req_abc124",
    "timestamp": "2025-01-15T10:35:00Z"
  }
}
```

### Códigos de Status HTTP

| Código | Uso |
|--------|-----|
| 200 OK | Operação bem-sucedida (GET, PUT, PATCH) |
| 201 Created | Recurso criado (POST) |
| 202 Accepted | Requisição aceita para processamento assíncrono |
| 204 No Content | Operação bem-sucedida sem retorno (DELETE) |
| 400 Bad Request | Payload inválido |
| 401 Unauthorized | Token ausente ou inválido |
| 403 Forbidden | Sem permissão para ação |
| 404 Not Found | Recurso não encontrado |
| 409 Conflict | Conflito (ex: registro duplicado) |
| 422 Unprocessable Entity | Validação de negócio falhou |
| 429 Too Many Requests | Rate limit excedido |
| 500 Internal Server Error | Erro no servidor |
| 503 Service Unavailable | Serviço temporariamente indisponível |

### Paginação

```http
GET /curation/records?page=2&limit=20&sort=-createdAt
```

**Response:**
```json
{
  "data": [ /* ... */ ],
  "pagination": {
    "page": 2,
    "limit": 20,
    "totalPages": 50,
    "totalItems": 1000,
    "hasNext": true,
    "hasPrev": true
  },
  "links": {
    "self": "/curation/records?page=2&limit=20",
    "first": "/curation/records?page=1&limit=20",
    "prev": "/curation/records?page=1&limit=20",
    "next": "/curation/records?page=3&limit=20",
    "last": "/curation/records?page=50&limit=20"
  }
}
```

### Filtros e Buscas

```http
GET /curation/records?status=pending&family=Euphorbiaceae&region=Amazônia&search=medicinal
```

**Operadores:**
- Igualdade: `?field=value`
- Maior que: `?field[gt]=value`
- Menor que: `?field[lt]=value`
- Contém: `?field[contains]=value`
- In: `?field[in]=val1,val2`
- Busca full-text: `?search=query`

### Versionamento

**Estratégia: URL Path Versioning**

```
https://api.etnoknowledge.org/v1/records
https://api.etnoknowledge.org/v2/records
```

**Por quê?**
- Simplicidade: Fácil de entender e usar
- Caching: Diferentes versões podem ter políticas distintas
- Documentação: Versões separadas facilitam documentar mudanças

**Política de Depreciação:**
1. Novas features em versão nova (v2)
2. Versão anterior mantida por 12 meses
3. Warnings nos headers 6 meses antes de desativar:
   ```http
   Deprecation: Sun, 11 Jan 2026 23:59:59 GMT
   Link: <https://docs.etnoknowledge.org/api/v2/migration>; rel="deprecation"
   ```

---

## Padrões GraphQL (Apresentação)

### Schema Principal

```graphql
schema {
  query: Query
  mutation: Mutation
}

type Query {
  # Espécies
  species(id: ID!): Species
  searchSpecies(input: SpeciesSearchInput!): SpeciesConnection!

  # Usos
  uses(category: UseCategory, limit: Int = 20): [TraditionalUse!]!

  # Regiões
  regions: [Region!]!
  regionStats(regionId: ID!): RegionStats!

  # Estatísticas
  stats: GlobalStats!
}

input SpeciesSearchInput {
  name: String
  family: String
  region: String
  useCategory: UseCategory
  country: String
  limit: Int = 20
  offset: Int = 0
}

type SpeciesConnection {
  edges: [SpeciesEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type SpeciesEdge {
  node: Species!
  cursor: String!
}

type Species {
  id: ID!
  scientificName: String!
  commonNames: [String!]!
  family: String
  kingdom: String
  verified: Boolean!

  # Relações
  uses: [TraditionalUse!]!
  regions: [Region!]!
  images: [Image!]!

  # Metadados
  lastUpdated: DateTime!
}

type TraditionalUse {
  id: ID!
  description: String!
  category: UseCategory!
  preparationMethod: String
  community: String
  source: Source!
}

enum UseCategory {
  MEDICINAL
  FOOD
  RITUAL
  CONSTRUCTION
  CRAFT
  OTHER
}

type Region {
  id: ID!
  name: String!
  country: String!
  coordinates: Coordinates
  speciesCount: Int!
}

type Coordinates {
  latitude: Float!
  longitude: Float!
}

type Source {
  type: SourceType!
  reference: String
  doi: String
  year: Int
}

enum SourceType {
  PRIMARY
  SECONDARY
}
```

### Exemplo de Query

```graphql
query GetSpeciesDetails($id: ID!) {
  species(id: $id) {
    scientificName
    commonNames
    family
    verified

    uses {
      category
      description
      preparationMethod
      source {
        type
        reference
        year
      }
    }

    regions {
      name
      country
      coordinates {
        latitude
        longitude
      }
    }

    images {
      url
      caption
      credit
    }
  }
}
```

**Variables:**
```json
{
  "id": "507f1f77bcf86cd799439011"
}
```

### Paginação (Cursor-based)

```graphql
query SearchSpecies($name: String!, $first: Int!, $after: String) {
  searchSpecies(
    input: { name: $name, limit: $first, offset: 0 }
  ) {
    edges {
      node {
        id
        scientificName
        commonNames
      }
      cursor
    }
    pageInfo {
      hasNextPage
      endCursor
    }
    totalCount
  }
}
```

### Error Handling

GraphQL retorna 200 OK mesmo com erros, detalhando no payload:

```json
{
  "data": null,
  "errors": [
    {
      "message": "Species not found",
      "locations": [{ "line": 2, "column": 3 }],
      "path": ["species"],
      "extensions": {
        "code": "NOT_FOUND",
        "speciesId": "invalid_id"
      }
    }
  ]
}
```

---

## Autenticação e Autorização

### REST APIs (Aquisição e Curadoria)

**Autenticação: JWT (JSON Web Tokens)**

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Fluxo:**
1. Login: `POST /auth/login` → retorna access token (15min) + refresh token (7 dias)
2. Requisições: incluir access token no header
3. Renovação: `POST /auth/refresh` com refresh token

**JWT Payload:**
```json
{
  "sub": "user_id",
  "email": "user@example.com",
  "role": "curator",
  "permissions": ["read", "write", "approve"],
  "iat": 1642512000,
  "exp": 1642512900
}
```

**Autorização: RBAC (Role-Based Access Control)**

Middleware verifica role e permissions:
```javascript
const authorize = (requiredPermissions) => {
  return (req, res, next) => {
    const userPermissions = req.user.permissions;
    const hasPermission = requiredPermissions.every(p =>
      userPermissions.includes(p)
    );

    if (!hasPermission) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    next();
  };
};

// Uso
router.post('/records/:id/approve', authorize(['approve']), approveRecord);
```

### GraphQL API (Apresentação)

**Autenticação: API Keys (para desenvolvedores)**

```http
X-API-Key: etno_live_abc123def456
```

**Tipos de API Keys:**
1. **Public** (read-only, rate limit 100 req/min)
2. **Authenticated** (maior rate limit 1000 req/min)

**Rate Limiting:**
- Por IP (público sem key): 10 req/min
- Por API key: conforme plano

---

## Rate Limiting

**Estratégia: Token Bucket Algorithm**

```javascript
const rateLimiter = {
  public: { limit: 10, window: 60 },    // 10 req/min
  authenticated: { limit: 100, window: 60 }, // 100 req/min
  developer: { limit: 1000, window: 60 }  // 1000 req/min
};
```

**Headers de Response:**
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1642512960
```

**Quando limite excedido (429):**
```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Limite de requisições excedido",
    "retryAfter": 45
  }
}
```

---

## Documentação

### REST: OpenAPI (Swagger)

**Ferramentas:**
- Swagger UI para documentação interativa
- Geração automática via anotações no código

**Exemplo (Express + swagger-jsdoc):**
```javascript
/**
 * @openapi
 * /acquisition/records:
 *   post:
 *     summary: Criar novo registro
 *     tags: [Acquisition]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             $ref: '#/components/schemas/RecordInput'
 *     responses:
 *       201:
 *         description: Registro criado com sucesso
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/Record'
 */
router.post('/records', createRecord);
```

**URL da Documentação:**
- Swagger UI: `https://api.etnoknowledge.org/docs`
- OpenAPI JSON: `https://api.etnoknowledge.org/openapi.json`

### GraphQL: Introspection + Playground

**GraphQL Playground:**
- URL: `https://api.etnoknowledge.org/graphql`
- Auto-documentação via introspection
- Exploração interativa do schema

**Documentação adicional:**
- Descrições nos tipos do schema:
```graphql
"""
Representa uma espécie botânica com conhecimento tradicional associado.
"""
type Species {
  """
  Nome científico da espécie (nomenclatura binomial).
  """
  scientificName: String!
}
```

---

## Padrão de Ingestão (Aquisição)

### Schema de Dados Padrão

Para facilitar integração de múltiplas fontes, definimos um schema JSON:

**JSON Schema:** `https://api.etnoknowledge.org/schemas/record-v1.json`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["species", "uses", "source"],
  "properties": {
    "species": {
      "type": "object",
      "required": ["scientificName"],
      "properties": {
        "scientificName": { "type": "string" },
        "commonNames": {
          "type": "array",
          "items": { "type": "string" }
        },
        "family": { "type": "string" }
      }
    },
    "uses": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "required": ["category", "description"],
        "properties": {
          "category": {
            "type": "string",
            "enum": ["medicinal", "food", "ritual", "construction", "craft", "other"]
          },
          "description": { "type": "string" }
        }
      }
    }
  }
}
```

### Endpoint de Validação

Antes de enviar, fontes externas podem validar:

```http
POST /acquisition/validate
Content-Type: application/json

{
  "species": { ... },
  "uses": [ ... ]
}
```

**Response:**
```json
{
  "valid": false,
  "errors": [
    {
      "field": "uses[0].category",
      "message": "Invalid category"
    }
  ]
}
```

---

## Monitoramento e Observabilidade

### Logging

Toda requisição deve logar:
```json
{
  "timestamp": "2025-01-15T10:30:00Z",
  "requestId": "req_abc123",
  "method": "POST",
  "path": "/acquisition/records",
  "statusCode": 201,
  "responseTime": 45,
  "userId": "user_123",
  "userAgent": "curl/7.64.1",
  "ip": "192.168.1.1"
}
```

### Métricas (Prometheus)

- `http_requests_total{method, path, status}`
- `http_request_duration_seconds{method, path}`
- `api_errors_total{code, endpoint}`

### Health Check

```http
GET /health
```

**Response:**
```json
{
  "status": "healthy",
  "version": "1.2.3",
  "uptime": 86400,
  "dependencies": {
    "database": "healthy",
    "redis": "healthy",
    "elasticsearch": "degraded"
  }
}
```

---

## Consequências

### Positivas
- APIs consistentes e previsíveis
- Boa experiência para desenvolvedores
- Flexibilidade para clientes públicos (GraphQL)
- Documentação auto-gerada

### Negativas
- Complexidade de manter dois estilos (REST + GraphQL)
- GraphQL pode ser over-engineering para casos simples

### Mitigações
- Compartilhar lógica de negócio entre REST e GraphQL (services layer)
- Treinamento de equipe em ambas tecnologias

## Referências

- [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines)
- [GraphQL Best Practices](https://graphql.org/learn/best-practices/)
- [OpenAPI Specification](https://swagger.io/specification/)

## Data de Revisão

**Próxima Revisão**: 6 meses após implementação
