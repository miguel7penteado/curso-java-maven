# Curso Maven - Formação JAVA Raiz

A ferramenta **MAVEN** para geração de projetos `JAVA`.

------------------------------------------------------------------------

### 1. Introdução

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

### 2. Ciclos de Vida

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

### 3. Plugins dos Ciclos de Vida

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

1. `Processar recursos` -> resources:resources
2. `Compilar` -> compiler:compile
3. `Processar recursos de teste` -> resources:testResources
4. `Compilar teste` -> compiler:testCompile
5. `Testar` -> surefire:test
6. `Empacotar` -> ejb:ejb or ejb3:ejb3 or jar:jar or par:par or rar:rar or war:war
7. `Instalar` -> install:install
8. `Entregar` -> deploy:deploy

Cada uma destas etapas será executada por cada um destes plugins:

O plugin **Resources**
O plugin **Compiler** 
O plugin **Surefire** 
O plugin **Failsafe** 
O plugin **Verifier** 
O plugin **Install**
O plugin **Deploy**

#### 3.1 Ciclo de Vida Default - Processando Recursos - `Plugin Resources`
------------------------------------------------------------------------
O plug-in `Resources` **copia arquivos dos diretórios de recursos de 
entrada para um diretório de saída**. O plug-in tem **três objetivos**,
que são diferentes apenas na forma como os diretórios de recursos e saída 
são especificados.
* **resources** - copia recursos que fazem parte do código fonte 
principal para o diretório de saída principal;
* **testResources** - copia recursos que fazem parte do código-fonte 
do teste no diretório de saída do teste;
* **copy-resources** - copia arquivos de recursos arbitrários para um
diretório de saída, exigindo que especifiquemos os arquivos de entrada e o 
diretório de saída;

Exemplo: 
```xml
<build>
	<plugins>
		<plugin>
			<artifactId>maven-resources-plugin</artifactId>
			<version>3.0.2</version>
			<configuration>
				<outputDirectory>saida-arquivos</outputDirectory>
				<resources>
					<resource>
						<directory>entrada-arquivos</directory>
						<excludes>
							<exclude>*.png</exclude>
						</excludes>
						<filtering>true</filtering>
					</resource>
				</resources>
			</configuration>
		</plugin>
	</plugins>
</build>
		
```
#### 3.2 Ciclo de Vida Default - Compilação de código - `Plugin Compiler`
------------------------------------------------------------------------
O plug-in do `compilador` é usado para compilar o código-fonte de um 
projeto Maven. Este plug-in tem dois objetivos, que já estão vinculados a
fases específicas do ciclo de vida padrão:

* **compile** - compila os principais arquivos de origem;
* **testCompile** - compila arquivos de origem de teste;

Exemplo:
```xml
<build>
	<plugins>
		<plugin>
		    <artifactId>maven-compiler-plugin</artifactId>
		    <version>3.7.0</version>
		    <configuration>
			    <source>1.8</source>
			    <target>1.8</target>
			    <compilerArgs>
				<arg>-Xlint:unchecked</arg>
			    </compilerArgs>
			    <-- outras customizações -->
		    </configuration>
		</plugin>
	</plugins>
</build>
```
Agora para evocar a compilação:
```sh
mvn -q clean compile exec:java -Dexec.mainClass="meu.pacote.NomeClassePrincipal"
```

#### 3.3 Ciclo de Vida Default - Teste do projeto - `Plugin surefire`
------------------------------------------------------------------------
O plugin `surefire` tem por objetivo testar o projeto utilizando 
frameworks **JUnit** e **TestNG** para isso. Por padrão, esse plug-in 
gera relatórios XML no diretório target / surefire-reports.
Por padrão, o surefire inclui automaticamente todas as classes de teste
cujo nome começa com **Test** ou termina com **Test**, **Tests** ou 
**TestCase**.

```xml
<build>
	<plugins>
		<plugin>
		    <artifactId>maven-surefire-plugin</artifactId>
		    <version>2.21.0</version>
		    <configuration>
			<excludes>
			    <exclude>DataTest.java</exclude>
			</excludes>
			<includes>
			    <include>DataCheck.java</include>
			</includes>
		    </configuration>
		</plugin>
	</plugins>
</build>
```

#### 3.4 Ciclo de Vida Default - Teste do projeto - `Plugin failsafe`
------------------------------------------------------------------------
O plug-in à prova de falhas é usado para testes de integração de um 
projeto. Tem dois objetivos:
* **integration-test** - executa testes de integração. Esse objetivo está 
vinculado à fase de teste de integração por padrão;
* **verify** - verifique se os testes de integração foram aprovados. Esse
objetivo está vinculado à fase de verificação por padrão;
