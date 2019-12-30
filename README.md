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

Todas as instruções que o maven deve executar são definidas em um arquivo
formatado em xml chamado **POM.xml**

------------------------------------------------------------------------

### Ciclos de Vida

Uma vez que, como dissemos, **todos os trabalhos são por plug-ins**
existem plugins chamados `core` que são responsáveis pelos **ciclos de 
vida** da construção do seu projeto.
O Maven define **três** ciclos de vida de construção: 

* **default** ou padrão;
* **site** para criação de um site;
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

------------------------------------------------------------------------

### Plugins dos Ciclos de Vida

Todos os plugin utilizados devem ser declarados no **pom.xml** em uma 
sessão xml chamada ```<build>...</build>```. (Vamos exemplificar um 
arquivo **pom.xml** completo posteriormente.)

```xml
...
<build>
    <plugins>
        <plugin>
            <artifactId>plugin1</artifactId>
            <version>versao_do_plugin1</version>
        </plugin>
        <plugin>
            <artifactId>plugin2</artifactId>
            <version>versao_do_plugin2</version>
        </plugin>      
        <plugin>
            <artifactId>plugin3</artifactId>
            <version>versao_do_plugin3</version>
        ...  
        </plugin>      
    </plugins>
</build>
...
```
As ligações internas do ciclo de vida **default** dependem do valor do 
elemento de empacotamento da POM. Por uma questão de brevidade, 
examinaremos as ligações dos tipos de empacotamentos mais comuns: 
**jar** e **war** .

Aqui está uma lista dos objetivos associados a cada fase do ciclo de 
vida **default** no formato "fase -> plugin: objetivo":





