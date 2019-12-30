# Curso Maven - Formação JAVA Raiz

A ferramenta **MAVEN** para geração de projetos `JAVA`.

------------------------------------------------------------------------

### Introdução

**Maven** é a ferramenta de construção mais usada no mundo `Java`. 
Principalmente, é apenas uma estrutura de execução de plug-in na qual 
**todos os trabalhos são implementados por plug-ins**. Neste tutorial, 
apresentaremos uma introdução aos principais plug-ins do Maven, 
fornecendo links para outros tutoriais com foco no que esses 
plug-ins podem fazer e como seus objetivos estão vinculados aos 
**ciclos de vida** da construção.

------------------------------------------------------------------------

### Ciclos de Vida

Uma vez que, como dissemos, **todos os trabalhos são por plug-ins**
existem plugins chamados `core` que são responsáveis pelos **ciclos de 
vida** da construção do seu projeto.
O Maven define **três** ciclos de vida de construção: 

* **default** ou padrão;
* **site **;
* **clean** ou limpeza.

Cada ciclo de vida é composto de **várias fases**, que **são executadas na 
ordem especificada no comando mvn**.
  O ciclo de vida mais importante é o **default**, responsável por todas as 
etapas do processo de construção, da validação do projeto à implantação
do pacote.
   O ciclo de vida do **site** é responsável por criar um site, mostrando 
informações relacionadas ao Maven do projeto. 
   O ciclo de vida **clean** cuida da remoção dos arquivos gerados na 
compilação anterior.
   Muitas fases nos três ciclos de vida são automaticamente vinculadas 
aos objetivos dos plugins principais. Os artigos referenciados 
abordarão esses objetivos e as ligações internas em detalhes.

