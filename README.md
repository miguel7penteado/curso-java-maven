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

#### 3.1.1 Ciclo de Vida Default - Processando Recursos - `Plugin Resources`
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
#### 3.1.2 Ciclo de Vida Default - Compilação de código - `Plugin Compiler`
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

#### 3.1.3 Ciclo de Vida Default - Teste do projeto - `Plugin surefire`
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

#### 3.1.4 Ciclo de Vida Default - Teste do projeto - `Plugin failsafe`
------------------------------------------------------------------------
O plug-in `failsafe` é usado para testes de integração de um 
projeto. Tem dois objetivos:
* **integration-test** - executa testes de integração. Esse objetivo está 
vinculado à fase de teste de integração por padrão;
* **verify** - verifique se os testes de integração foram aprovados. Esse
objetivo está vinculado à fase de verificação por padrão;

Este plugin executa métodos em classes de teste, assim como o 
plugin surefire. Podemos configurar os dois plugins de maneiras 
semelhantes. No entanto, existem algumas diferenças cruciais entre eles. 

Primeiro, diferentemente do surefire (consulte este artigo) incluído no
super pom.xml, o plug-in à prova de falhas com seus objetivos deve ser 
explicitamente especificado no pom.xml para fazer parte de um ciclo de 
vida da construção:

```xml
<build>
	<plugins>
		<plugin>
		    <artifactId>maven-failsafe-plugin</artifactId>
		    <version>2.21.0</version>
		    <executions>
			<execution>
			    <goals>
				<goal>integration-test</goal>
				<goal>verify</goal>
			    </goals>
			    <configuration>
				...
			    </configuration>
			</execution>
		    </executions>
		</plugin>
	</plugins>
</build>
```
Segundo, o plug-in à prova de falhas executa e verifica testes usando 
objetivos diferentes. Uma falha de teste na fase de teste de integração 
não interrompe o processo, permitindo que a fase de pós-teste
de integração seja executada, onde são executadas operações de limpeza.

Testes com falha, se houver, são relatados apenas durante a fase de 
verificação, depois que o ambiente de teste de integração foi desativado 
corretamente.

#### 3.1.5 Ciclo de Vida Default - Teste do projeto - `Plugin verifier`
------------------------------------------------------------------------
O plug-in `verifier` tem apenas um objetivo: 
* **verify**. Esse objetivo verifica a existência ou não de arquivos
e diretórios, opcionalmente verificando o conteúdo do arquivo em relação
a uma expressão regular;

Apesar do nome, o objetivo de verificação está 
vinculado à **fase de teste** de integração por padrão, em vez da fase
de verificação.

```xml
<build>
	<plugins>
		<plugin>
		    <artifactId>maven-verifier-plugin</artifactId>
		    <version>1.1</version>
		    <configuration>
			<verificationFile>input-resources/verifications.xml</verificationFile>
		    </configuration>
		    <executions>
			<execution>
			    <goals>
				<goal>verify</goal>
			    </goals>
			</execution>
		    </executions>
		</plugin>
	</plugins>
</build>		
```
------------------------------------------------------------------------
O local padrão do arquivo de verificação é 
**src/test/verifier/verifications.xml**. Nós devemos definir um valor 
para o parâmetro **notificationFile** se quisermos usar outro arquivo.

```xml
<verifications
  xmlns="http://maven.apache.org/verifications/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/verifications/1.0.0 
  http://maven.apache.org/xsd/verifications-1.0.0.xsd">
    <files>
        <file>
            <location>input-resources/baeldung.txt</location>
            <contains>Welcome</contains>
        </file>
    </files>
</verifications>
```
Este arquivo de verificação confirma que existe um arquivo chamado 
**input-resources/baeldung.txt** e que contém a palavra Bem-vindo. 
Já adicionamos esse arquivo antes, portanto, a execução do objetivo é bem-sucedida.

#### 3.1.6 Ciclo de Vida Default - Instalação do projeto - `Plugin verifier`
------------------------------------------------------------------------
O plug-in `install` adiciona **artefatos** ao repositório
local. Esse plug-in está incluído no super POM, portanto, um POM 
não precisa incluí-lo explicitamente.

O objetivo mais notável desse plug-in é a **instalação**, que 
é vinculada à fase de instalação por padrão.

Outros objetivos são o arquivo de instalação usado para instalar
automaticamente artefatos externos no repositório local e ajudar a mostrar
informações no próprio plug-in.

Na maioria dos casos, o plug-in de instalação não precisa de nenhuma 
configuração personalizada. É por isso que não vamos nos aprofundar 
neste plugin.

#### 3.1.7 Ciclo de Vida Default - Implantação do projeto - `Plugin deploy`
------------------------------------------------------------------------
O plug-in `deploy` implementa a fase de implantação, **enviando artefatos
para um repositório remoto para compartilhar com outros desenvolvedores**.

Além do próprio artefato, esse plug-in garante que todas as informações 
associadas, como POMs, **metadados ou valores de hash**, estejam corretas.

O plug-in `deploy` especificado no super POM, portanto, não é 
necessário adicionar esse plug-in ao POM.

Este plugin possui dois objetivos:
* **deploy**  vinculado à fase de implantação por padrão;
* **deploy-file** implementando um artefato em um repositório remoto;

#### 3.2.1 Ciclo de Vida Site - Criação do site - `Plugin site`
------------------------------------------------------------------------
Por padrão, o ciclo de vida **site** do Maven tem duas fases 
vinculadas aos objetivos do plug-in `site`: a fase **site** vinculada 
ao objetivo do site e a fase de **implantação do site** está vinculada
ao objetivo de implantação.

* **site** - gere um site para um único projeto; o site gerado mostra 
apenas informações sobre os artefatos especificados no POM;
* **deploy** - implanta o site gerado na URL especificada no elemento 
distributionManagement do POM;

Além de site e implantação, o plug-in `site` tem vários outros objetivos
para personalizar o conteúdo dos arquivos gerados e controlar o 
processo de implantação.

#### 3.2.2 Ciclo de Vida Clean - Limpeza do projeto - `Plugin clean`
------------------------------------------------------------------------
O ciclo de vida **limpeza** possui apenas uma fase chamada **clean** que 
é automaticamente vinculada ao único objetivo do plug-in `clean`. 
Este objetivo pode, portanto, ser executado com o comando `mvn clean`.

O plug-in limpo já está incluído no super POM, portanto, podemos usá-lo
sem especificar nada no POM do projeto.

Esse plug-in, como o nome indica, limpa os arquivos e diretórios gerados
durante a compilação anterior. 
Por padrão, o plug-in **remove o diretório de destino**.

```xml
<build>
	<plugins>
		<plugin>
		    <artifactId>maven-clean-plugin</artifactId>
		    <version>3.0.0</version>
		    <configuration>
			<filesets>
			    <fileset>
				<directory>output-resources</directory>
			    </fileset>
			</filesets>
		    </configuration>
		</plugin>
	</plugins>
</build>			
```

------------------------------------------------------------------------

### 4. Plugins dos Ciclos de Vida `Site`

