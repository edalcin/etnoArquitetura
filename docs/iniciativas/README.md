# Iniciativas Relacionadas ao Conhecimento Tradicional e Sociobiodiversidade

Este diretório contém resumos de iniciativas brasileiras relacionadas à gestão de conhecimento tradicional, sociobiodiversidade e patrimônio genético. Estas iniciativas servem como fontes de dados, informações e referências arquiteturais para o projeto etnoArquitetura.

## Visão Geral

As iniciativas documentadas compartilham objetivos comuns:
- Proteção e valorização do conhecimento tradicional associado à biodiversidade
- Aplicação dos princípios CARE (Collective Benefit, Authority to Control, Responsibility, Ethics)
- Aplicação dos princípios FAIR (Findable, Accessible, Interoperable, Reusable)
- Interoperabilidade com padrões internacionais (DarwinCore, PlinianCore)
- Participação e controle das comunidades tradicionais sobre seus dados
- Conformidade com a Lei 13.123/2015 (Lei da Biodiversidade)

## Iniciativas Documentadas

### 1. Projeto GEF 'Entre-Ciências'
**Arquivo**: [MCTI-GEF/Projeto-GEF_Entre-Ciencias_RESUMO.md](MCTI-GEF/Projeto-GEF_Entre-Ciencias_RESUMO.md)

**Coordenação**: MCTI (Ministério da Ciência, Tecnologia e Inovação)
**Período**: 2025-2029 (60 meses)
**Financiamento**: US$ 6,2 milhões (GEF) + US$ 74,7 milhões (cofinanciamento)

**Destaques**:
- Maior iniciativa integrada envolvendo PIPCTAFs (Povos Indígenas, Povos e Comunidades Tradicionais e Agricultores Familiares)
- Desenvolvimento de ferramentas de coleta e compartilhamento no SiBBr
- Portal territorial para cada comunidade participante
- Sistema de rastreabilidade com DOI para conjuntos de dados
- Programa de capacitação (FormarBio) e bolsas de pesquisa
- Controles de acesso baseados em princípios CARE
- Interoperabilidade com SISGEN e Plataforma Territórios Tradicionais

**Relevância Arquitetural**:
- Requisitos funcionais detalhados para coleta, transformação, armazenamento e compartilhamento
- Especificações de controles de acesso e rastreabilidade
- Modelo de governança de dados com PIPCTAFs
- Padrões DarwinCore e PlinianCore

### 2. Rede de Conhecimentos sobre a Sociobiodiversidade
**Arquivo**: [redeConhecimento/SEI_022081189_Nota_Tecnica_1_RESUMO.md](redeConhecimento/SEI_022081189_Nota_Tecnica_1_RESUMO.md)

**Coordenação**: CNPT/ICMBio em parceria com UFSC
**Período**: 2010-2025 (desenvolvimento contínuo)

**Destaques**:
- Integração de dados dispersos sobre sociobiodiversidade
- Banco de Dados Sociobio (BD Sociobio)
- Laboratórios especializados: ECOHE, ARANDU, OBSERVA (UFSC)
- Grupo de pesquisa CNPq registrado
- Plataforma virtual para compartilhamento
- Programa de capacitação FormarBio
- Considerações éticas sobre dados sensíveis

**Relevância Arquitetural**:
- Modelo de integração de fontes dispersas
- Tratamento de dados sensíveis (práticas espirituais, territórios sagrados)
- Experiência de 15 anos em gestão de dados etnobotânicos
- Protocolos de compartilhamento com comunidades

### 3. SISGEN - Plano de Interoperabilidade
**Arquivo**: [SISGEN/plano_de_trabalho_sisgen_mma_-_sibbr_rnp_RESUMO.md](SISGEN/plano_de_trabalho_sisgen_mma_-_sibbr_rnp_RESUMO.md)

**Coordenação**: MMA com RNP e SiBBr
**Período**: 2024-2025 (13 meses)
**Financiamento**: BID (Banco Interamericano de Desenvolvimento)

**Destaques**:
- Instalação do IPT (Integrated Publishing Toolkit)
- Padronização em DarwinCore e PlinianCore
- Manual de Interoperabilidade para patrimônio genético
- Capacitação em FAIR e CARE
- Templates para entrada/saída de dados
- Publicação de datasets de biodiversidade

**Relevância Arquitetural**:
- Especificações técnicas de interoperabilidade
- Uso do IPT como ferramenta de publicação
- Processo de transformação de dados para padrões internacionais
- Integração com GBIF
- Arquitetura para repartição de benefícios

### 4. Useflora - Banco de Dados Etnobotânicos
**Arquivo**: [Useflora/TCC _Patricia_Ferrari_RESUMO.md](Useflora/TCC _Patricia_Ferrari_RESUMO.md)

**Desenvolvimento**: ECOHE/UFSC
**Período**: 2005-2020 (acumulação de dados e desenvolvimento)
**URL**: www.useflora.ufsc.br

**Destaques**:
- Banco de dados relacional MySQL com 41 variáveis
- 3.359 registros implementados (3.011 espécies, 25 referências, 322 informações)
- Sistema de cadastro comunitário inovador
- Dois níveis de acesso (público com ocultação, restrito com CRUD completo)
- Duas formas de entrada de dados (formulários web, importação de arquivos TBS)
- Stack tecnológico: PHP, MySQL, HTML, JavaScript, Bootstrap
- Exportação para CSV

**Relevância Arquitetural**:
- Modelo de dados completo e testado (ER diagram)
- Implementação prática de controles de acesso
- Interface para cadastro comunitário
- Estratégias de importação em lote
- Ocultação de campos sensíveis para usuários públicos
- Modelo de empoderamento comunitário através do registro

## Temas Transversais

### Princípios de Governança de Dados

**CARE (para dados indígenas e tradicionais)**:
- **C**ollective Benefit: Benefício coletivo para as comunidades
- **A**uthority to Control: Autoridade das comunidades sobre seus dados
- **R**esponsibility: Responsabilidade no uso dos dados
- **E**thics: Ética na coleta, uso e compartilhamento

**FAIR (para dados científicos)**:
- **F**indable: Dados encontráveis com identificadores persistentes
- **A**ccessible: Acessíveis através de protocolos padronizados
- **I**nteroperable: Interoperáveis com outros sistemas
- **R**eusable: Reutilizáveis com licenças claras

### Padrões de Dados

**DarwinCore (DwC)**:
- Padrão internacional para registros de ocorrência de espécies
- Mantido pela TDWG (Biodiversity Information Standards)
- Usado pelo GBIF e redes relacionadas
- Campos essenciais: scientificName, decimalLatitude, decimalLongitude, eventDate, recordedBy

**PlinianCore**:
- Extensão do DarwinCore para informações sobre espécies
- Inclui tipos de uso (alimentar, medicinal, ritual, etc.)
- Parte utilizada (folha, semente, casca, etc.)
- Forma de preparo e uso

### Ferramentas Comuns

**IPT (Integrated Publishing Toolkit)**:
- Ferramenta GBIF para publicação de dados de biodiversidade
- Transforma dados para DarwinCore/PlinianCore
- Gera arquivos Darwin Core Archive (DwC-A)
- Atribui identificadores persistentes (DOI)

**SiBBr (Sistema de Informação sobre a Biodiversidade Brasileira)**:
- Nó brasileiro do GBIF
- Coordenado pelo MCTI, operado pela RNP
- Mais de 28 milhões de registros
- Portal: https://sibbr.gov.br

**SISGEN (Sistema Nacional de Gestão do Patrimônio Genético)**:
- Cadastro de acessos ao patrimônio genético e conhecimento tradicional
- Gestão de repartição de benefícios
- Portal: https://sisgen.gov.br

### Marco Legal

**Lei 13.123/2015 (Lei da Biodiversidade)**:
- Regulamenta acesso ao patrimônio genético brasileiro
- Regulamenta acesso ao conhecimento tradicional associado
- Estabelece repartição justa e equitativa de benefícios
- Cria o SISGEN como sistema de controle

**Decretos Relacionados**:
- Decreto 6.040/2007: Política Nacional de Povos e Comunidades Tradicionais
- Decreto 8.750/2016: Conselho Nacional dos Povos e Comunidades Tradicionais
- Decreto 7.747/2012: PNGATI (Gestão Territorial de Terras Indígenas)

## Aspectos Arquiteturais Comuns

### Requisitos Funcionais Identificados

1. **Coleta de Dados**
   - Aplicativos mobile/web
   - Suporte a mídia (fotos, áudio, vídeo)
   - Funcionalidade offline
   - Georreferenciamento automático
   - Formulários adaptáveis por comunidade

2. **Transformação e Padronização**
   - Conversão para DarwinCore/PlinianCore
   - Validação taxonômica
   - Curadoria de dados
   - Normalização de nomenclatura

3. **Armazenamento**
   - Banco de dados relacional (MySQL/PostgreSQL)
   - Repositório em nuvem
   - Versionamento de dados
   - Backup e recuperação

4. **Controle de Acesso**
   - Múltiplos níveis (público, restrito, privado)
   - Autenticação (gov.br, CAFe, customizada)
   - Autorização granular por campo
   - Ocultação de dados sensíveis

5. **Rastreabilidade**
   - Identificadores persistentes (DOI)
   - Log de acessos e downloads
   - Termos de uso e licenciamento
   - Tracking de citações

6. **Interoperabilidade**
   - APIs REST
   - Padrões DwC/PlinianCore
   - IPT para publicação
   - Integração com GBIF/SiBBr

7. **Visualização**
   - Portais web por território
   - Mapas interativos
   - Dashboards de monitoramento
   - Cruzamento de dados territoriais

### Requisitos Não-Funcionais

1. **Segurança**
   - Criptografia de dados sensíveis
   - Proteção contra biopirataria
   - Auditoria de acessos

2. **Escalabilidade**
   - Suporte a milhões de registros
   - Crescimento de usuários
   - Distribuição geográfica

3. **Usabilidade**
   - Interface para diferentes níveis de letramento digital
   - Suporte multilíngue (português + línguas indígenas)
   - Acessibilidade (WCAG)

4. **Confiabilidade**
   - Disponibilidade alta
   - Backup regular
   - Recuperação de desastres

## Desafios Comuns Identificados

1. **Conectividade**: Acesso à internet em áreas remotas (mitigação: apps offline, internet via satélite)
2. **Capacitação**: Necessidade de formação contínua em tecnologia e governança de dados
3. **Confiança**: Histórico de biopirataria gera resistência ao compartilhamento
4. **Sustentabilidade**: Garantir continuidade após fim de financiamentos de projetos
5. **Complexidade Institucional**: Coordenação entre múltiplos órgãos e níveis de governo
6. **Diversidade Cultural**: Adaptar sistemas a diferentes protocolos e cosmovisões

## Lições Aprendidas

1. **Participação Essencial**: CLPI (Consentimento Livre, Prévio e Informado) desde o início
2. **Controle Comunitário**: PIPCTAFs devem decidir sobre seus próprios dados
3. **Capacitação Ampla**: Não apenas tecnologia, mas também direitos e governança
4. **Flexibilidade**: Protocolos adaptáveis às especificidades culturais
5. **Interoperabilidade**: Essencial para sustentabilidade de longo prazo
6. **Coprodução**: Conhecimento tradicional e científico em pé de igualdade

## Próximos Passos Sugeridos

Para o projeto etnoArquitetura, as iniciativas documentadas sugerem:

1. **Análise Comparativa**: Mapear semelhanças e diferenças entre as abordagens
2. **Arquitetura de Referência**: Criar modelo baseado nas melhores práticas identificadas
3. **Requisitos Consolidados**: Lista unificada de requisitos funcionais e não-funcionais
4. **Protótipo**: Desenvolver MVP baseado nas lições aprendidas
5. **Governança**: Definir modelo de governança de dados inspirado nas iniciativas
6. **Interoperabilidade**: Garantir compatibilidade com SiBBr, SISGEN e GBIF desde o início

## Contatos e Recursos

### Instituições-chave
- **MCTI**: Coordenação Geral de Ecossistemas e Biodiversidade
- **ICMBio/CNPT**: Centro Nacional de Pesquisa e Conservação da Sociobiodiversidade Associada a Povos e Comunidades Tradicionais
- **UFSC**: Universidade Federal de Santa Catarina (ECOHE, ARANDU, OBSERVA)
- **RNP**: Rede Nacional de Ensino e Pesquisa
- **IEB**: Instituto Internacional de Educação do Brasil

### Sistemas e Plataformas
- SiBBr: https://sibbr.gov.br
- GBIF: https://www.gbif.org
- SISGEN: https://sisgen.gov.br
- Useflora: www.useflora.ufsc.br
- Plataforma Territórios Tradicionais: https://territoriostradicionais.mpf.mp.br

### Padrões e Ferramentas
- DarwinCore: https://dwc.tdwg.org
- PlinianCore: https://github.com/tdwg/PlinianCore
- IPT: https://www.gbif.org/ipt
- Princípios CARE: https://www.gida-global.org/care

---

**Última Atualização**: Janeiro 2025
**Projeto**: etnoArquitetura - Sistema de Informação de Conhecimento Tradicional
