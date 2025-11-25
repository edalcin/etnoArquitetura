# Banco de Dados Etnobotânicos: Useflor@ - Resumo Executivo

## Informações Gerais

**Título**: Banco de dados etnobotânicos: construção de uma ferramenta de armazenamento e proteção de informações sobre a sociobiodiversidade

**Autora**: Patrícia Aparecida Ferrari

**Instituição**: Universidade Federal de Santa Catarina (UFSC)

**Laboratório**: ECOHE - Laboratório de Ecologia Humana e Etnobotânica

**Ano**: 2020

**Tipo**: Trabalho de Conclusão de Curso

**URL do Banco**: www.useflora.ufsc.br

**Hospedagem**: Servidor UFSC

## Contexto e Motivação

### Laboratório ECOHE

**Histórico**:
- Criado em 30 de agosto de 2004
- 15 anos de atuação até o desenvolvimento do Useflor@
- Coordenadores: Prof. Natalia Hanazaki e Prof. Nivaldo Peroni

**Foco de Pesquisa**:
- Interação entre pessoas e recursos naturais
- Padrões de conhecimento e uso de recursos
- Interfaces entre populações locais/tradicionais e áreas de conservação
- Percepção sobre recursos biológicos e ecossistemas
- Metodologias de coleta e análise de dados em ecologia humana

### Necessidade Identificada

**Problema**:
- 15 anos de dados coletados
- Dados armazenados em planilhas isoladas (Excel®)
- Crescente volume de informações
- Necessidade de relacionamento entre bases de informações
- Informações dispersas em diferentes formatos

**Demanda Específica**:
- Revisão bibliográfica de Liporacci (2014): compilou grande volume de dados etnobotânicos sobre plantas medicinais e alimentícias da Mata Atlântica e Caatinga
- Dados de Ehlert (2018) sobre espécies úteis
- Múltiplas demandas de estruturação em 2019

## Objetivos do Projeto

### Objetivo Geral

Construir e implementar, através de ferramentas computacionais, um **banco de dados biológicos informacional e relacional** para armazenamento de informações etnobotânicas e de sociobiodiversidade coletadas no ECOHE-UFSC, visando:
- Proteção de informações sobre conhecimento tradicional associado
- Armazenamento organizado
- Auxílio em análises e metanálises

### Objetivos Específicos

1. Sistematizar os principais tipos de informações levantadas em pesquisas etnobotânicas
2. Construir um SGBD Relacional
3. **Implementar método para registro comunitário** de novos dados
4. Disponibilizar com áreas de acesso restrito a integrantes cadastrados
5. Disponibilizar partes com acesso público
6. Desenvolver método de solicitação de cadastro (login e senha)
7. Testar funcionamento com dados de Liporacci (2014)

## Marco Legal e Conceitual

### Legislação Relevante

**Lei 13.123/2015**:
- Acesso ao patrimônio genético
- Conhecimento tradicional associado
- Repartição de benefícios

**Definição Legal de CTA**:
> "informação ou prática de população indígena, comunidade tradicional ou agricultor tradicional sobre propriedades ou usos do patrimônio genético"

**Formas de Reconhecimento (Art. 8 § 3º)**:
- Publicações científicas
- **Registros em cadastros ou bancos de dados**
- Inventários culturais

### Conceitos Fundamentais

**Sociobiodiversidade**:
> "cada vez mais a diversidade cultural humana - incluindo a diversidade de línguas, crenças e religiões, práticas de manejo de solo, expressões artísticas, tipos de alimentação e diversos outros atributos humanos - é interpretada como sendo um componente significativo da biodiversidade" (ALBAGLI, 1998)

**Conhecimento Tradicional Associado**:
- Resultado de co-evolução entre populações e ambientes
- Experiências adquiridas e adaptadas às necessidades locais
- Desenvolvido através do contato com o meio ambiente ao longo de gerações
- Conhecimento ecológico local

**Brasil Megadiverso**:
- Cerca de 20% do número total de espécies mundiais
- Mais de 200 Povos Indígenas
- Diversos Povos e Comunidades Locais

## Arquitetura do Banco de Dados Useflor@

### Estrutura Técnica

**Modelo**: Banco de Dados Relacional (BDR)

**SGBD**: MySQL

**Linguagens de Programação**:
- **SQL** (Structured Query Language) - para o SGBD
- **HTML** - interface gráfica
- **PHP** (Hypertext Preprocessor) - lógica servidor
- **JavaScript** - funcionalidades complexas
- **CSS** - estilos

**Frameworks e Ferramentas**:
- **Bootstrap** - framework front-end
- **FontAwesome** - ícones
- **Workbench** - modelagem visual do banco
- **Visual Studio** - editor de código
- **FileZilla** - transferência de arquivos para servidor
- **phpMyAdmin** - administração do MySQL

**Operações CRUD**:
- Create (Criar)
- Read (Ler/Consultar)
- Update (Atualizar)
- Delete (Deletar)

### Modelo Entidade-Relacionamento

**Metodologia**:
- Diagrama ER (Entidade-Relacionamento) desenvolvido no Workbench
- Todas as tabelas recebem ID único (chave primária)
- Relações entre tabelas através de chaves estrangeiras

**Por que ID único**:
- Nenhuma informação é isenta de repetição
- Duas espécies podem ter mesmo nome popular
- Mesmas informações de localização
- ID elimina confusão por repetição nas buscas

### Estrutura de Dados

**4 Categorias Principais**:

1. **Espécies**
2. **Referências**
3. **Informações**
4. **Domesticação** (em desenvolvimento)

**Total de Variáveis**: 41 campos

#### 1. Categoria "Espécies" (6 campos)

| Campo | Descrição | Tipo de Entrada |
|-------|-----------|-----------------|
| Identificação | Completa ou incompleta até nível de espécie | Seleção de categoria |
| Família | Família botânica | Texto (APG IV) |
| Gênero | Gênero botânico | Texto |
| Nome científico | Nome científico conferido | Texto (Flora do Brasil) |
| Autor | Autor que nomeou a espécie | Texto (IPNI) |
| Sinonímias | Espécies com sinonímias | Número da espécie cadastrada |

#### 2. Categoria "Referências" (7 campos)

| Campo | Descrição | Tipo de Entrada |
|-------|-----------|-----------------|
| Inserção | Como a referência foi localizada | Texto |
| Referência | Referência bibliográfica completa | Texto (BIBTEX) |
| Citação | Referência abreviada | Texto (autor + ano) |
| Coleta | Técnica utilizada para coleta | Texto (método usado) |
| Amostragem | Tipo de amostragem | Texto (método usado) |
| Data | Data de inserção | Data (formato brasileiro) |
| DOI | Identificador DOI | Sequência numérica |

#### 3. Categoria "Informações" (28 campos)

**Etnobotânica**:
- Nome popular
- Uso popular (categoria)
- Detalhes (texto descritivo)
- Destinação econômica
- Parte usada

**Botânica**:
- Origem (exótica ou nativa)
- Hábito (forma de vida)
- Voucher (número de tombamento)

**Localização**:
- Latitude
- Longitude
- Datum
- Estado
- Cidade
- Bacia hidrográfica

**Conservação**:
- Território (território tradicional)
- Área (área protegida)
- Tipos (categoria de proteção)
- Esfera (esfera de gestão)

#### 4. Categoria "Domesticação"

Em desenvolvimento, não abordada em profundidade no TCC.

### Hierarquia de Cadastro

**Ordem Obrigatória**:

```
1. ESPÉCIE
   ↓
2. REFERÊNCIA
   ↓
3. INFORMAÇÕES
```

**Por quê?**
- Informações sempre fazem referência a uma espécie
- Informações sempre fazem referência a uma referência bibliográfica
- Relacionamento via chaves primárias (IDs)

## Sistema de Acesso e Segurança

### Dois Níveis de Acesso

#### Acesso Público (sem login)

**Funcionalidades**:
1. **Busca por Informações** (com ocultações):
   - Campos ocultos: "detalhes de uso" e "parte usada"
   - Proteção de conhecimento sensível
2. **Registro Comunitário** (formulário)
3. **Contato** (formulário)
4. **Solicitação de Login** (formulário)

**Filtros de Busca Disponíveis**:
- Nome científico
- Categoria de uso popular
- Grupo ecológico
- Bioma
- Categoria de área protegida
- Autor do estudo

#### Acesso Restrito (com login)

**Funcionalidades Adicionais**:
1. **Busca Completa** (sem ocultações)
2. **Exportar Resultados** (formato CSV)
3. **Acesso às 4 Áreas Principais**:
   - Espécies
   - Referências
   - Informações
   - Domesticação
4. **Manipulação de Dados (CRUD)**:
   - Consultar
   - Adicionar
   - Editar
   - Apagar

**Cadastro de Novos Logins**:
- Atualmente manual via administrador
- Através do phpMyAdmin
- Formulário de solicitação disponível
- Análise de aprovação baseada em:
  - Interesse em contribuição
  - Vínculos
  - Área de atuação

**Controle de Acesso**:
- Sistema de rastreio por data e horário
- Identificação do último usuário que atualizou
- DBA (Database Administrator) com acesso total
- Senha criptografada e intransferível

### Funcionalidades de Cadastro

#### Método 1: Formulários Web

**Características**:
- Inserção de uma informação por vez
- Interface visual intuitiva
- Validação de dados
- Edição imediata após inserção
- Relacionamento automático via seleção

**Ordem de Preenchimento**:
1. Formulário de Espécie
2. Formulário de Referência
3. Formulário de Informações (seleciona espécie e referência)

#### Método 2: Importação de Arquivos

**Formato**: TBS (Tab Separated Values)

**Por que TBS?**:
- Melhor aceitação de caracteres especiais
- Acentos preservados
- Vírgulas no conteúdo não causam problemas
- CSV (comma-separated) problemático com vírgulas em referências

**Capacidade Testada**:
- Cerca de 300 dados de uma vez
- Valores superiores: lentidão ou falha de importação

**Desafios**:
- Usuário precisa organizar dados na ordem exata dos campos
- IDs devem ser preenchidos manualmente
- Relações devem ser feitas manualmente
- Processo trabalhoso e sujeito a erros
- Útil para alimentar banco com dados pré-existentes

**Avaliação**:
> "Essa relação manual é como se o usuário estivesse fazendo um trabalho para o banco"

**Permanência em Avaliação**:
- Útil inicialmente para migração de dados existentes
- Pode ser reavaliada com base na experiência do usuário

### Formulários de Contato

**Via Google Forms** (solução temporária):

1. **Solicitação de Login**:
   - Identificação
   - Justificativa
   - Área de atuação
   - Análise para aprovação

2. **Contato**:
   - Comunicação com administradores
   - Dúvidas e sugestões

3. **Registro Comunitário**:
   - Primeiro contato de comunidades
   - Interesse em registrar conhecimento

**Limitação Atual**:
- Não há notificação automática de novos formulários
- Requer verificação manual constante
- Servidor ainda não configurado para envio/recebimento de e-mails
- Funcionalidade pode ser facilmente adicionada via SETIC/UFSC

## Registro Comunitário: Inovação Fundamental

### Conceito e Importância

**Proposta Diferencial**:
> "uma das grandes funcionalidades e diferenciais desse banco de dados é o desenvolvimento de uma ferramenta que permita o registro comunitário"

**Objetivo**:
- Permitir que comunidades locais registrem seu próprio conhecimento
- Valorização dos conhecedores e especialistas locais
- Participação ativa no processo de documentação
- Proteção através do registro

### Fundamentação Legal

**Lei 13.123/15, Art. 8º - Direitos das Comunidades**:

1. **Ter indicada a origem** do acesso ao conhecimento tradicional em todas as publicações
2. **Impedir terceiros não autorizados** de:
   - Utilizar, realizar testes, pesquisa ou exploração
   - Divulgar, transmitir ou retransmitir dados
3. **Perceber benefícios** pela exploração econômica

### Benefícios Esperados

**Para as Comunidades**:
- Documentação do conhecimento
- Controle sobre acesso às informações
- Definição de níveis de acesso (restrito, parcial, público)
- Papel de destaque e valorização
- Proteção contra apropriação indevida

**Para os Jovens**:
- Ferramenta digital pode aumentar interesse
- Estimular interlocução entre jovens e idosos
- Reverter efeitos negativos da modernização
- Manter viva a transmissão de conhecimento

### Processo de Registro (em Desenvolvimento)

**Etapas Planejadas**:
1. Contato inicial via formulário
2. Envio de formulário detalhado de informações
3. Assinatura de termo de comprometimento
4. Recebimento de termo de reconhecimento
5. Acesso para inserção de dados

**Controle pela Comunidade**:
- Decisão sobre confiabilidade dos dados
- Escolha do nível de acesso:
  - Totalmente restrito
  - Acessível por outros grupos tradicionais
  - Aberto para pesquisa
  - Outras possibilidades

**Discussões Necessárias**:
- Como incluir membros da comunidade no acesso restrito
- Manipulação de dados por membros da comunidade
- Deve agregar opinião dos detentores
- Sistema feito **com** e **para** as comunidades

## Dados Implementados e Testes

### Dados Inseridos

**Até o momento do TCC**:
- **Total**: 3.359 dados
  - **3.011 Espécies**: todas de Liporacci (2014) + Ehlert (2018)
  - **25 Referências**: dentre 92 da revisão
  - **322 Informações**: relacionadas a 7 referências

**Fonte Principal**: Revisão bibliográfica de Liporacci (2014)
- 90 artigos analisados
- Plantas medicinais e alimentícias
- Biomas: Mata Atlântica (57 artigos) e Caatinga (33 artigos)

### Resultados dos Testes

**Comunidades Identificadas**:
1. **Caiçaras**: etnia identificada
2. **Imigrantes Italianos**: sem etnia formal, autodenominação

**Observação Importante**:
- Trabalhos da década de 90 não traziam identificação dos grupos
- Atualmente obrigação legal (Lei 13.123/15)
- Ausência dificulta busca por filtro de etnia

**Distribuição por Bioma**:

**Caiçaras - Florestas tropicais e subtropicais úmidas** (192 espécies):
- 98 espécies: categoria "ME: Medicinal humano, terapêutico"
- 64 espécies: categoria "AL: Alimentício"

**Imigrantes Italianos - Pradarias e savanas tropicais** (130 espécies):
- 130 espécies: categoria "ME: Medicinal humano, terapêutico"
- 0 espécies: categoria "AL: Alimentício"

**Interpretação**:
- Caiçaras eram o segmento mais estudado nas florestas tropicais
- Proporcionalmente menos artigos sobre plantas alimentícias
- Estudos etnobotânicos historicamente voltados às medicinais

**Espécies Mais Citadas**:

1. **Citrus aurantium**: 7 citações
   - 6 vezes entre Caiçaras
   - 1 vez entre Imigrantes Italianos
   - Usos: medicinal e alimentício
   - Diferentes partes utilizadas

2. **Manihot esculenta**: 6 citações
   - Todas relacionadas a Caiçaras
   - 4 autores diferentes
   - Categoria: alimentício

### Validação do Sistema

**Demonstração de Efetividade**:
> "Através dos filtros, o usuário pode chegar a qual espécie aparece mais em determinado bioma, ou qual a categoria de uso que determinada espécie para o grupo ecológico que ele estuda e assim responder perguntas da sua pesquisa ou gerar novas discussões."

## Aspectos Técnicos Relevantes

### Evolução do Desenvolvimento

**Processo Iterativo**:
- Cerca de 15 reuniões ao longo de 2019
- Membros do laboratório e colaboradores
- Levantamento e discussão da estrutura
- Detalhamento de campos de informação
- Múltiplas alterações no DER

**Dificuldades Identificadas**:
- Planilhas são tabelas únicas sem divisões
- Dificulta visualização de relacionamentos
- Demandas surgiram durante desenvolvimento
- Necessidade de ajustes contínuos

**Importância das Reuniões**:
> "é importante ressaltar a importância dos encontros e discussões em grupo. Isso possibilitou que a estrutura interna do banco de dados se desenvolvesse de maneira bastante completa e detalhada"

### Decisões de Design

**Formulários vs. Arquivos**:

**Formulários** (Recomendado):
- ✅ Inserção guiada
- ✅ Validação automática
- ✅ Relacionamento via seleção
- ✅ IDs gerados automaticamente
- ✅ Menos propenso a erros

**Arquivos** (Útil para Migração):
- ⚠️ Requer organização manual exata
- ⚠️ IDs devem ser preenchidos manualmente
- ⚠️ Relações manuais
- ⚠️ Trabalhoso e propenso a erros
- ✅ Útil para dados pré-existentes em volume

**Campos de Seleção vs. Texto Livre**:
- Categorias padronizadas: seleção
- Informações descritivas: texto livre
- Equilíbrio entre padronização e flexibilidade

### Padrões e Referências Utilizados

**Nomenclatura Botânica**:
- **APG IV**: sistema de classificação de famílias
- **Flora do Brasil**: formato de nomes científicos
- **IPNI**: abreviação de nomes de autores

**Referências Bibliográficas**:
- **BIBTEX**: formato para referências completas
- Citação abreviada: autor + ano

**Georreferenciamento**:
- Coordenadas em graus decimais
- Datum especificado
- IBGE 2000 para bacias hidrográficas

### Identificação Incompleta

**Solução Prática**:
- Quando identificação apenas até gênero (ex: *Avicennia* sp.)
- Nome científico = epíteto incompleto + autor do estudo
- Exemplo: *Avicennia* sp. Silva & Andrade, 2006
- Filtro específico para identificação completa/incompleta

**Justificativa**:
- Facilitar busca
- Melhoria na pesquisa
- Não perder informações válidas

### Exportação de Dados

**Formato**: CSV (Comma Separated Values)

**Funcionalidade Restrita**:
- Somente usuários logados
- Resultados de buscas podem ser exportados
- Permite manipulação externa
- Análises em outras ferramentas

## Iniciativas Relacionadas

### Rede de Conhecimentos sobre a Sociobiodiversidade

**Parceria**: CNPT/ICMBio e UFSC

**Objetivo**:
> "estabelecer uma rede de conhecimentos sobre a Sociobiodiversidade brasileira, integrando bases de dados existentes no país e informações relevantes em uma plataforma única de acesso livre"

**Relação com Useflor@**:
- Propósitos semelhantes de integração
- Livre acesso
- Colaboração estratégica
- Useflor@ contribui para perspectiva maior

### Outras Iniciativas Mencionadas

**People's Biodiversity Register (PBR) - Índia**:
- Criado em decorrência da CDB
- Registrar conhecimentos de comunidades locais
- Proteger dados de usos relacionados à biodiversidade
- Comunicação entre detentores e facilitadores
- Repartição de benefícios
- Referência: GADGIL (1996), PADMANABHAN (2008)

**Bancos de Dados Existentes**:
- **Flora Digital**: fotos e informações de plantas do sul do Brasil
- **Flora do Brasil 2020**: algas, plantas e fungos do Brasil
- **SiBBr**: Sistema de Informação sobre a Biodiversidade Brasileira
- **SisGen**: Sistema Nacional de Gestão do Patrimônio Genético e do Conhecimento Tradicional Associado

## Desafios e Riscos

### Proteção de Conhecimento Tradicional

**Riscos Identificados**:

1. **Apropriação Indevida** (Biopirataria):
   - Materiais biológicos e genéticos
   - Conhecimentos tradicionais
   - Sem acordo com leis ou consentimento

2. **Interesse Comercial**:
   - 25% dos medicamentos alopáticos de princípios ativos vegetais
   - 75% dos princípios ativos identificados via conhecimentos tradicionais
   - Aumento de 400% na possibilidade de reconhecer compostos bioativos
   - Interesse de indústrias farmacêuticas, químicas e agrícolas

3. **Exposição de Dados Sensíveis**:
   - Dados sobre recursos naturais valiosos
   - Informações que podem gerar interesse econômico
   - Necessidade de proteção

### Medidas de Segurança Implementadas

**1. Sistema de Login**:
- Controle de acesso
- Rastreabilidade de alterações
- Data e horário de modificações
- Identificação de usuários

**2. Divisão de Acesso**:
- Público: busca limitada
- Restrito: manipulação completa
- Ocultação de campos sensíveis para público

**3. Análise de Cadastros**:
- Aprovação manual de novos logins
- Critérios de avaliação
- Vínculos e área de atuação verificados

**Limitações Reconhecidas**:
> "deve ser realizado um levantamento mais profundo da funcionalidade dos níveis de acesso"

**Necessidades Futuras**:
- Sistema de hierarquia de acesso mais complexo
- Mais subdivisões de permissões
- Usuários com diferentes níveis de manipulação
- Mecanismos mais eficazes alinhados com legislação
- Diálogo com códigos de ética em pesquisa etnobiológica

### Desafios Técnicos

**1. Volume de Dados**:
- 15 anos de acumulação
- Múltiplos formatos
- Tabelas isoladas
- Necessidade de relacionamento

**2. Importação de Dados Legados**:
- Processo trabalhoso
- Propenso a erros manuais
- IDs manuais complexos
- Lentidão com grandes volumes

**3. Usabilidade**:
- Adaptação de usuários
- Interface visual a ser melhorada
- Filtros de busca a serem aperfeiçoados
- Visualização de resultados

**4. Manutenção**:
- Necessidade de administrador ativo
- Atualizações contínuas
- Correções de erros
- Aprovação de cadastros

## Considerações sobre Ética e Legislação

### Marcos Legais Aplicáveis

**Convenção sobre Diversidade Biológica (CDB - 1992)**:
> "respeitar, preservar e manter o conhecimento, inovações e práticas das comunidades locais e populações indígenas com estilos de vida tradicionais relevantes à conservação e utilização sustentável da diversidade biológica"

**Brasil**:
- Ratificação pelo Congresso Nacional em 1994
- Soberania sobre diversidade biológica
- Repartição justa e equitativa de benefícios

**Lei 13.123/2015**:
- Acesso ao patrimônio genético
- Proteção do conhecimento tradicional
- Repartição de benefícios

**Decreto 8.772/2016**:
- Regulamentação da Lei 13.123/2015

**Decreto 6.040/2007**:
- Política Nacional de Desenvolvimento Sustentável dos Povos e Comunidades Tradicionais (PNPCT)

### Princípios Éticos

**Responsabilidade com Culturas Envolvidas**:
- Estudos etnobotânicos devem ser éticos
- Respeito ao ser humano
- Respeito ao meio ambiente
- Reconhecimento de direitos legais

**Pesquisas Participativas**:
- Reconhecimento do direito de participar
- Decisões sobre conservação dos ecossistemas
- Dependência direta de recursos e benefícios

**Papel Social da Ciência**:
- Conservação da biodiversidade e ecossistemas
- Valorização cultural
- Sobrevivência cultural de povos e comunidades
- Justiça cognitiva

### Importância do Registro

**Registro como Proteção**:
> "destaca-se o papel importante que bancos de dados possuem na garantia dos direitos de propriedade intelectual destas comunidades"

**Formas de Reconhecimento (Lei 13.123/15, Art. 8 § 3º)**:
- Publicações científicas
- **Registros em cadastros ou bancos de dados**
- Inventários culturais

**Valorização**:
> "A participação das comunidades no registro de dados pode contribuir para a valorização das mesmas" (ZANK, 2015)

## Lições Aprendidas e Reflexões

### Sobre o Desenvolvimento

**1. Importância da Participação Coletiva**:
- Reuniões frequentes essenciais
- Múltiplas perspectivas enriquecem
- Estrutura mais completa e detalhada
- Ajustes contínuos necessários

**2. Dificuldade de Migração de Planilhas**:
- Planilhas não mostram relacionamentos
- Visualização de BDR é diferente
- Adaptação mental necessária
- Processo de aprendizado

**3. Iteração Necessária**:
- DER passou por múltiplas versões
- Campos ajustados durante desenvolvimento
- Demandas emergentes incorporadas
- Flexibilidade importante

### Sobre Registro Comunitário

**Potencial Transformador**:
> "o objetivo é que essa informação esteja de alguma forma documentada e possa ser distribuída a sociedade, e a comunidade tradicional detentora do conhecimento tradicional possa assumir um papel de destaque e valorização neste processo"

**Autonomia e Controle**:
- Comunidade define confiabilidade
- Comunidade escolhe níveis de acesso
- Decisão centralizada nos detentores
- Empoderamento social

**Inclusão de Jovens**:
- Ferramenta digital pode atrair interesse
- Combate desinteresse em conhecimentos tradicionais
- Estimula diálogo intergeracional
- Reverte efeitos negativos da modernização

**Necessidade de Co-construção**:
> "estas discussões devem agregar também a opinião dos detentores, para que este sistema possa ser feito com as comunidades e para as comunidades"

### Sobre Proteção de Dados

**Tensão Necessária**:
- Compartilhamento vs. Proteção
- Acesso público vs. Conhecimento sensível
- Ciência aberta vs. Apropriação indevida
- Equilíbrio delicado

**Medidas Parciais**:
- Sistema de login como primeira medida
- Reconhecimento de limitações
- Necessidade de aprimoramento
- Diálogo com legislação e ética

## Perspectivas Futuras

### Melhorias Planejadas

**Interface e Usabilidade**:
- Categorias de filtro mais específicas
- Resultado total de dados da busca
- Interface visual mais adequada
- Melhor visualização de resultados

**Sistema de Comunicação**:
- Contato, registro e login automatizados
- E-mail próprio do banco (via servidor UFSC)
- Notificações automáticas
- Menos dependência de administrador

**Registro Comunitário**:
- Segundo formulário detalhado
- Termos de compromisso
- Termos de reconhecimento
- **Verificar interesse e aceitação das comunidades**
- Interface dedicada para comunidades

**Ferramentas Complementares**:
- Apps de coleta remota
- Comunicação com o banco
- Auxílio nas coletas de campo
- Integração mobile

### Integrações Futuras

**Rede de Conhecimentos sobre Sociobiodiversidade**:
- Colaboração com CNPT/ICMBio
- Integração com outras bases
- Plataforma única de acesso livre
- Contribuição para rede nacional

**Outras Bases de Dados**:
- SiBBr
- Flora do Brasil
- SisGen
- Possíveis integrações internacionais

## Relevância para Arquitetura de Dados de Conhecimento Tradicional

### Modelo de Referência Prático

O Useflor@ oferece um **caso concreto** de implementação de banco de dados etnobotânicos com as seguintes características valiosas:

**1. Banco de Dados Relacional Funcional**:
- MySQL com estrutura testada
- DER bem documentado
- 41 variáveis organizadas
- Sistema CRUD completo

**2. Múltiplos Níveis de Acesso**:
- Público vs. Restrito
- Ocultação de campos sensíveis
- Sistema de login
- Rastreabilidade

**3. Registro Comunitário Inovador**:
- Participação ativa das comunidades
- Controle pelos detentores
- Escolha de níveis de acesso
- Empoderamento social

**4. Conformidade Legal**:
- Lei 13.123/2015
- CDB
- PNPCT
- Registro como forma de reconhecimento

### Desafios Identificados

**1. Complexidade de Importação**:
- Dados legados em planilhas
- Processo manual propenso a erros
- Relacionamentos não triviais
- Necessidade de limpeza

**2. Segurança e Proteção**:
- Biopirataria como risco real
- Interesse comercial em dados
- Necessidade de múltiplos níveis
- Aprimoramento contínuo necessário

**3. Usabilidade**:
- Diferentes perfis de usuários
- Comunidades com letramento digital variado
- Interface deve ser intuitiva
- Formulários vs. importação

**4. Participação Comunitária**:
- Necessidade de co-construção
- Opinião dos detentores fundamental
- Termos e acordos necessários
- Processo em desenvolvimento

### Tecnologias Validadas

**Stack Tecnológico Funcional**:
- MySQL (SGBD confiável e gratuito)
- PHP + HTML + JavaScript (amplamente suportado)
- Bootstrap (framework agiliza desenvolvimento)
- Workbench (modelagem visual eficaz)
- Servidor UFSC (hospedagem institucional)

**Formatos de Dados**:
- BIBTEX para referências
- TBS para importação (melhor que CSV)
- CSV para exportação
- Coordenadas em graus decimais

**Padrões de Nomenclatura**:
- APG IV (famílias)
- Flora do Brasil (nomes)
- IPNI (autores)
- IBGE (bacias hidrográficas)

### Insights Arquiteturais

**1. Chaves Primárias Essenciais**:
- IDs únicos eliminam ambiguidade
- Nomes, locais, usos podem repetir
- Relacionamentos via IDs robustos

**2. Hierarquia de Cadastro**:
- Espécie → Referência → Informações
- Ordem lógica e necessária
- Garante integridade referencial

**3. Formulários Preferíveis**:
- Guiam o usuário
- Validação automática
- Menos erros
- Melhor experiência

**4. Importação para Migração**:
- Útil para dados existentes
- Complementar, não principal
- Requer preparação cuidadosa
- Documentação essencial

**5. Divisão de Acesso Graduada**:
- Não é binário (tudo ou nada)
- Campos podem ser ocultados seletivamente
- Níveis intermediários importantes
- Controle granular futuro

### Questões em Aberto

**1. Como Escalar o Registro Comunitário?**
- Muitas comunidades, poucos administradores
- Automação vs. Controle
- Validação de identidade
- Suporte técnico

**2. Como Garantir Sustentabilidade?**
- Dependência de administrador ativo
- Manutenção de longo prazo
- Atualizações tecnológicas
- Recursos institucionais

**3. Como Medir Sucesso?**
- Número de registros?
- Comunidades participantes?
- Proteção efetiva?
- Uso em pesquisas?

**4. Como Integrar com Outras Iniciativas?**
- APIs para interoperabilidade
- Padrões comuns
- Rede de conhecimentos
- Escala nacional/internacional

## Dados Quantitativos

**Dados Implementados** (até 2020):
- 3.359 registros totais
- 3.011 espécies cadastradas
- 25 referências (de 90 analisadas)
- 322 informações relacionadas
- 2 comunidades identificadas (Caiçaras e Imigrantes Italianos)

**Origem dos Dados**:
- Revisão bibliográfica de Liporacci (2014): 90 artigos
  - 57 artigos Mata Atlântica
  - 33 artigos Caatinga
- Compilação de Ehlert (2018)

**Desenvolvimento**:
- Cerca de 15 reuniões ao longo de 2019
- 15 anos de dados do ECOHE a serem integrados
- 41 variáveis em 4 categorias

**Capacidade**:
- ~300 dados por importação sem lentidão
- Volumes superiores: lentidão ou falha

## Conclusões

### Sobre o Useflor@

**Efetividade Demonstrada**:
> "O Useflor@ está se mostrando uma ferramenta eficaz para o armazenamento estruturado de informações de etnobotânica e sociobiodiversidade"

**Funcionalidade Testada**:
- Sistema CRUD completo funcional
- Buscas relacionais efetivas
- Exportação operacional
- Múltiplos níveis de acesso implementados

### Sobre Bancos de Dados Etnobotânicos

**Necessidade Reconhecida**:
> "Os Bancos de Dados Relacionais permitem que o grande volume de dados biológicos possa ser organizado, relacionado e compartilhado a mais usuários evitando os BD isolados e podendo contribuir com pesquisas futuras"

**Benefícios Múltiplos**:
- Avanços nas pesquisas
- Auxílio em análise e coleta
- Relação de dados
- Aproximação com comunidades
- Empoderamento social
- Conservação da biodiversidade

### Sobre Registro Comunitário

**Potencial Transformador**:
- Aproxima comunidades das tomadas de decisão
- Valorização de conhecedores locais
- Proteção através do registro
- Empoderamento na conservação

**Necessidade de Desenvolvimento**:
- Co-construção com comunidades
- Validação com detentores
- Processos participativos
- Diálogo contínuo

### Recomendações Gerais

**Para Projetos Similares**:

1. **Envolvimento Coletivo Essencial**:
   - Reuniões frequentes
   - Múltiplas perspectivas
   - Ajustes iterativos

2. **Modelagem Cuidadosa Inicial**:
   - Tempo investido em DER compensa
   - Relacionamentos bem definidos
   - Chaves primárias para tudo

3. **Formulários como Método Principal**:
   - Importação como complemento
   - Guiar usuário reduz erros
   - Experiência mais fluida

4. **Segurança em Camadas**:
   - Login como base
   - Múltiplos níveis futuro
   - Rastreabilidade importante
   - Aprimoramento contínuo

5. **Participação Comunitária Desde o Início**:
   - Não é apenas técnico
   - Diálogo fundamental
   - Co-construção necessária
   - Respeito aos detentores

## Referências Importantes do TCC

**Etnobotânica e Conhecimento Tradicional**:
- ALBUQUERQUE (2005) - Etnobotânica
- BALICK & COX (1997) - Plantas e pessoas
- BERKES (2017) - Conhecimento ecológico local
- HANAZAKI (2003, 2006) - Etnobotânica e conservação
- DIEGUES (2000) - Sociobiodiversidade

**Bancos de Dados**:
- ELMASRI et al. (2005) - Sistemas de banco de dados
- DUBOIS (2008) - Bancos de dados
- KOFLER (2006) - MySQL

**Legislação**:
- Lei 13.123/2015 - Lei da Biodiversidade
- Decreto 8.772/2016 - Regulamentação
- CDB (1992) - Convenção sobre Diversidade Biológica
- Decreto 6.040/2007 - PNPCT

**Iniciativas Relacionadas**:
- GADGIL (1996) - People's Biodiversity Register
- PADMANABHAN (2008) - PBR na Índia
- MAGALHÃES et al. (2010) - Automação de coleções biológicas

**Estudos de Caso**:
- LIPORACCI (2014) - Revisão bibliográfica MA e Caatinga
- EHLERT (2018) - Espécies úteis
- NASCIMENTO (2016) - LAPOGEdb

---

**Documento Original**: TCC _Patricia_Ferrari.pdf

**Título Completo**: Banco de dados etnobotânicos: construção de uma ferramenta de armazenamento e proteção de informações sobre a sociobiodiversidade

**Instituição**: UFSC - Universidade Federal de Santa Catarina

**Laboratório**: ECOHE - Laboratório de Ecologia Humana e Etnobotânica

**Ano**: 2020

**Elaborado para**: Projeto de Arquitetura - Sistema de Informação de Conhecimento Tradicional

**Data do Resumo**: Janeiro 2025
