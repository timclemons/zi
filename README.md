# Zi

Zi is a maven plugin for clojure. It does something similar to
[clojure-maven-plugin](http://github.com/talios/clojure-maven-plugin), but does
so differently.

It uses the maven pom `sourceDirectory` and `testSourceDirectory` settings to
locate source, which by default means that it uses `src/main/clojure` and
`src/test/clojure`. The goals should work with a `pom.xml` generated by
[leiningen](https://github.com/technomancy/leiningen).

From an implementation perspecitve, most of the goals are written in clojure.

This is alpha quality. It requires maven 3.0.3.

## Available goals

 * zi:resources
 * zi:testResources
 * zi:compile
 * zi:ritz
 * zi:swank-clojure
 * zi:test
 * zi:marginalia

## Install

Globally installing the plugin allows you to run the goals without modifying
a project's pom file.

To globally enable the zi plugin, you need to add `pluginGroup` and
`pluginRepository` configuration to your `~/.m2/settings.xml` file.

```xml
    <pluginGroups>
      <pluginGroup>org.cloudhoist.plugin</pluginGroup>
    </pluginGroups>
```

```xml
    <profiles>
      <profile>
        <id>clojure-dev</id>
        <pluginRepositories>
          <pluginRepository>
            <id>sonatype-snapshots</id>
            <url>http://oss.sonatype.org/content/repositories/releases</url>
          </pluginRepository>
        </pluginRepositories>
      </profile>
    </profiles>

    <activeProfiles>
      <activeProfile>clojure-dev</activeProfile>
    </activeProfiles>
```

To enable zi in a project pom, without globally enabling the plugin, you will
need to add a `pluginRepositories` entry to your pom:

```xml
    <pluginRepositories>
      <pluginRepository>
        <id>sonatype-snapshots</id>
        <url>http://oss.sonatype.org/content/repositories/releases</url>
      </pluginRepository>
    </pluginRepositories>
```

## Configuration

It uses the maven pom `sourceDirectory` and `testSourceDirectory` settings to
locate source, which by default means that it uses `src/main/clojure` and
`src/test/clojure`.

## Goals

### resources

The resources goal copies clojure source to the target. This is probably what
you need to make sure that your clj source files end up in your jar file.

```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.cloudhoist.plugin</groupId>
          <artifactId>zi</artifactId>
          <version>0.3.6</version>
          <executions>
            <execution>
              <id>default-resources</id>
              <phase>process-resources</phase>
              <goals>
                <goal>resources</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
      </plugins>
    </build>
```

### testResources

The testResources goal copies clojure test source to the target. This is
probably what you need to make sure that your clj source files end up in your
test-jar file.

```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.cloudhoist.plugin</groupId>
          <artifactId>zi</artifactId>
          <version>0.3.6</version>
          <executions>
            <execution>
              <id>default-test-resources</id>
              <phase>process-test-resources</phase>
              <goals>
                <goal>test-resources</goal>
              </goals>
            </execution>
          </executions>
        </plugin>
      </plugins>
    </build>
```

### compile

The compile goal AOT compiles clojure source.

```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.cloudhoist.plugin</groupId>
          <artifactId>zi</artifactId>
          <version>0.3.6</version>
          <executions>
            <execution>
              <id>default-compile</id>
              <goals>
                <goal>compile</goal>
              </goals>
            </execution>
          </executions>
          <configuration>
            <excludes>
              <exclude>**/test.clj</exclude>
            </excludes>
          </configuration>
        </plugin>
      </plugins>
    </build>
```

<table>
  <tr>
    <th>Property</th>
    <th>Variable</th>
    <th>Default</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>includes</td>
    <td>
    <td>**/*.clj</td>
    <td>A set of source patterns to include</td>
  </tr>
  <tr>
    <td>excludes</td>
    <td>
    <td></td>
    <td>A set of source patterns to exclude</td>
  </tr>
</table>

### ritz

The ritz goal starts a ritz server.

<table>
  <tr>
    <th>Property</th>
    <th>Variable</th>
    <th>Default</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>port</td>
    <td>clojure.swank.port</td>
    <td>4005</td>
    <td>The swank server port</td>
  </tr>
  <tr>
    <td>encoding</td>
    <td>clojure.swank.encoding</td>
    <td>iso-8859-1</td>
    <td>The swank encoding to use</td>
  </tr>
</table>

The ritz and swank-clojure goals both recognise sub-projects linked in a
checkouts directory, and adds their sources and resources to the the
classpath.

### swank-clojure

The swank-clojure goal starts a swank-clojure server.

<table>
  <tr>
    <th>Property</th>
    <th>Variable</th>
    <th>Default</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>port</td>
    <td>clojure.swank.port</td>
    <td>4005</td>
    <td>The swank server port</td>
  </tr>
  <tr>
    <td>encoding</td>
    <td>clojure.swank.encoding</td>
    <td>iso-8859-1</td>
    <td>The swank encoding to use</td>
  </tr>
</table>

### test

The test goal runs clojure.test tests.

<table>
  <tr>
    <th>Property</th>
    <th>Variable</th>
    <th>Default</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>initScript</td>
    <td>clojure.initScript</td>
    <td></td>
    <td>A clojure source string that is run before the tests</td>
  </tr>
</table>

### marginalia

The marginalia goal creates a marginalia annotated source page.

<table>
  <tr>
    <th>Property</th>
    <th>Variable</th>
    <th>Default</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>marginaliaTargetDirectory</td>
    <td></td>
    <td>${project.build.directory}</td>
    <td>The directory where marginali should write uberdoc.html</td>
  </tr>
</table>

## Zi, the builder

Zi was a builder in [northern mythology](http://www.pitt.edu/~dash/mbuilder.html#eckwadt).

## License

Licensed under [EPL](http://www.eclipse.org/legal/epl-v10.html)

Copyright 2011 Hugo Duncan.
