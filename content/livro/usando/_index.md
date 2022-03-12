---
title: "Usando o Apache Camel"
draft: false
weight: 4
---

Para utilizar o Apache Camel é necessário adicionar em um projeto as dependências básicas,
bem como as utilizadas pelo componente. Em um projeto Maven, a melhor forma de fazer isso é
importando o arquivo POM com a "Bill of Materials" (BOM) da versão do Camel.


Para um projeto usando o componente Camel Main, o mínimo necessário são os componentes `camel-core` e `camel-main`:

```xml
<dependencyManagement>
	<dependencies>
		<!-- Camel BOM -->
		<dependency>
			<groupId>org.apache.camel</groupId>
			<artifactId>camel-bom</artifactId>
			<version>3.14.2</version>
			<scope>import</scope>
			<type>pom</type>
		</dependency>
	</dependencies>
</dependencyManagement>

<dependencies>
	<dependency>
		<groupId>org.apache.camel</groupId>
		<artifactId>camel-core</artifactId>
	</dependency>
	<dependency>
		<groupId>org.apache.camel</groupId>
		<artifactId>camel-main</artifactId>
</dependency>
```

Dependendo do projeto ou do runtime utilizando, o `camel-main` pode não ser necessário.