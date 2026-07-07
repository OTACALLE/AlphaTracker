Alpha Tracker — Arquitetura, Filosofia e Diretrizes do Projeto
Introdução

O Alpha Tracker é uma plataforma de vigilância científica automatizada desenvolvida para identificar oportunidades inéditas na área de biologia estrutural.

O objetivo principal do sistema não é prever estruturas de proteínas. Seu objetivo é descobrir automaticamente quais proteínas recém-publicadas representam oportunidades científicas para futura predição estrutural, identificando casos em que existe uma sequência completa de aminoácidos, mas ainda não foi encontrada evidência pública de um modelo estrutural ou de uma publicação estrutural correspondente de alguém que rodou no AlphaFold.

O sistema foi concebido para funcionar continuamente, como um observatório científico, monitorando diversas fontes de informação em tempo real.

Filosofia do projeto

O Alpha Tracker deve atuar como um pesquisador automatizado.

Ele deve sempre ser de código aberto.

Seu comportamento esperado é semelhante ao de um cientista que, todos os dias, lê milhares de artigos, extrai novas sequências proteicas, consulta dezenas de bancos de dados e identifica oportunidades promissoras de pesquisa.

O sistema jamais deve gerar conclusões que não possam ser comprovadas.

Por exemplo, ele nunca deve afirmar:

"Ninguém rodou o AlphaFold para essa proteína."

Essa afirmação é impossível de verificar.

Em vez disso, o sistema sempre deve utilizar linguagem cientificamente correta, como:

"Não foi encontrada evidência pública de um modelo estrutural ou de uma publicação estrutural correspondente nas fontes consultadas."

Toda a arquitetura do Alpha Tracker foi construída em torno desse princípio.

Fluxo geral do sistema

O pipeline completo pode ser representado da seguinte forma:

Fontes científicas
        │
        ▼
Crawlers
        │
        ▼
Extração dos metadados
        │
        ▼
Extração da sequência FASTA
        │
        ▼
Validação da sequência
        │
        ▼
Consulta aos bancos científicos
        │
        ▼
Busca por evidências estruturais
        │
        ▼
Análise da proteína
        │
        ▼
Cálculo do Score
        │
        ▼
Banco de dados
        │
        ▼
Dashboard
        │
        ▼
Sistema de Alertas

Todo o sistema gira em torno desse pipeline.

Módulo 1 — Monitoramento científico

Este módulo monitora continuamente diversas fontes científicas.

Exemplos:

PubMed
PubMed Central
bioRxiv
medRxiv
arXiv
Nature(SE GRATUÍTO)
Science(SE GRATUÍTO)
Cell
eLife
Genome Biology

O monitoramento deve ocorrer continuamente.

Sempre que uma nova publicação surgir, ela deve ser adicionada ao pipeline.

O sistema deve evitar reprocessamentos utilizando cache e identificadores únicos.

Módulo 2 — Crawlers

Os crawlers são responsáveis por obter informações das diferentes plataformas.

Cada fonte deve possuir seu próprio conector independente.

Os crawlers nunca devem conter lógica científica.

Sua única responsabilidade é obter dados.

Módulo 3 — Parsers

Após o download dos artigos, o sistema interpreta os conteúdos.

O parser deve extrair:

título
DOI
autores
periódico
organismo
nome da proteína
sequência FASTA
data
URL

Quando existir XML ou texto estruturado, o parser deve utilizá-lo.

Caso contrário, utilizar técnicas de NLP.

Módulo 4 — Extração da sequência

A sequência de aminoácidos representa o principal dado do Alpha Tracker.

O sistema deve identificar automaticamente se existe uma sequência completa.

Sequências incompletas devem ser descartadas ou classificadas como baixa confiança.

Módulo 5 — Validação

Antes de prosseguir, verificar:

sequência válida
caracteres permitidos
comprimento
redundância
duplicatas
sequência parcial

Somente sequências suficientemente completas devem seguir para análise.

Módulo 6 — Consulta às bases científicas

Após obter a sequência, o sistema consulta automaticamente diversas bases.

Entre elas:

UniProt
GenBank
RefSeq
AlphaFold Protein Structure Database
Protein Data Bank
GitHub
Zenodo
Figshare
Dryad
OSF

O objetivo é reunir todas as evidências públicas existentes.

Módulo 7 — Busca por evidências estruturais

Este é o núcleo científico do projeto.

O sistema procura responder:

Existe modelo público?

Existe estrutura experimental?

Alguém que rodou no AlphaFold ou CollabFold publicou?

Existe publicação estrutural?

Existe preprint?

Existe modelo suplementar?

Todas essas respostas devem ser armazenadas.

Módulo 8 — Análise da proteína

Após reunir as evidências, o sistema realiza uma caracterização básica da proteína.

Entre elas:

comprimento(quando possível)
peso molecular(quando possível)
composição de aminoácidos(quando possível)
regiões de baixa complexidade(quando possível)
regiões desordenadas(quando possível)
peptídeo sinal(quando possível)
hélices transmembrana(quando possível)
coiled-coils(quando possível)
domínios (quando possível)

Isso só deve ocorrer sob ordem do usuário.

O sistema foi projetado para futuramente integrar ferramentas como:

InterPro
Pfam
SMART
BLAST
Módulo 9 — Score

O Score é uma estimativa quantitativa do potencial científico daquela proteína.

Ele considera diversos fatores.

Exemplos:

novidade
publicação recente
sequência completa
ausência de estrutura pública
importância biomédica
associação com doenças
prioridade para proteínas humanas
potencial farmacológico

O Score deve ser totalmente explicável.

Cada contribuição deve ficar registrada.

Módulo 10 — Banco de dados

Toda informação obtida deve ser persistida.

O banco deve armazenar:

Proteínas

Publicações

Sequências

Resultados

Consultas

Score

Alertas

Logs

Histórico

Nunca perder rastreabilidade.

Módulo 11 — Dashboard

O Dashboard representa a interface principal do Alpha Tracker.

Seu objetivo é permitir que o pesquisador acompanhe todo o pipeline em tempo real.

No futuro o Dashboard deverá funcionar como um verdadeiro centro de comando.

Deverá conter:

Painel Geral

Sistema online

Fontes monitoradas

O banco de dados

Tempo desde última atualização

Artigos processados

Proteínas encontradas

Alertas emitidos

Tempo médio de processamento

Monitor em tempo real

Mostrar exatamente o que o sistema está fazendo.

Exemplo:

Lendo PubMed

Extraindo FASTA

Consultando AlphaFold

Consultando UniProt

Calculando Score

Enviando alerta

Tabela principal

Cada proteína deve ocupar uma linha.

A tabela deverá conter colunas como:

Nome da proteína

Organismo

Comprimento

Número de aminoácidos

DOI

Revista

Data

Sequência FASTA

AlphaFold DB

PDB

Estrutura experimental

Publicação estrutural

Score

Prioridade

Estado

Justificativa

Ao clicar em uma proteína, abrir uma página completa com todas as informações coletadas.

Filtros

Permitir filtros extremamente detalhados.

Exemplos:

Proteínas humanas

Taupatias

PSP

Alzheimer

Parkinson

ELA

Score mínimo

Data

Revista

Organismo

Comprimento

Existência de AlphaFold

Existência de PDB

Existência de publicação estrutural

Timeline

Mostrar cronologicamente tudo o que ocorreu.

Estatísticas

Gráficos

Ranking

Distribuição dos Scores

Organismos

Revistas

Alertas

Módulo 12 — Sistema de Alertas

Quando uma proteína ultrapassar determinado Score, o sistema deve gerar automaticamente notificações.

Inicialmente:

Email

Podemos expandir no futuro.

Push Notification

O alerta deve conter todas as evidências encontradas e deixar claro que a conclusão se baseia apenas em fontes públicas consultadas.

Módulo 13 — Logs

Todo evento importante deve gerar logs.

Consultas

Falhas

Timeouts

Retries

Atualizações

Alertas

Mudanças no banco

Escalabilidade

Todo o projeto foi desenvolvido utilizando arquitetura modular.

Cada módulo deve ser independente.

Novos conectores devem ser adicionados sem modificar os módulos existentes.

O sistema deve ser facilmente expansível.

Objetivo de longo prazo

O Alpha Tracker não deve permanecer apenas como um monitor de artigos científicos.

O objetivo é transformá-lo em uma plataforma completa de inteligência científica para biologia estrutural.

No futuro ele deverá:

identificar automaticamente oportunidades de pesquisa;
priorizar proteínas com maior potencial científico;
integrar mais bancos de dados;
incorporar análises estruturais e funcionais mais avançadas;
oferecer um dashboard em tempo real para acompanhamento de todo o pipeline;
gerar relatórios completos e auditáveis;
permitir que pesquisadores acompanhem continuamente o estado das evidências públicas para proteínas recém-publicadas.

Ao modificar o projeto, preserve a arquitetura existente, mantenha a separação clara entre módulos, evite acoplamento desnecessário e documente todas as alterações. Novas funcionalidades devem ser integradas de forma incremental, mantendo compatibilidade com o código já implementado e priorizando robustez, rastreabilidade e rigor científico.
