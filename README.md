# ARM64 compatible dockerfile-maven-plugin

ARM64 build for dockerfile-maven-plugin

[![Maven Package](https://github.com/arm64-compat/dockerfile-maven-plugin/actions/workflows/maven-publish.yaml/badge.svg?branch=main)](https://github.com/arm64-compat/dockerfile-maven-plugin/actions/workflows/maven-publish.yaml)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Current version of dockerfile-maven-plugin doesn't support building `arm64` docker images.

Reference Issues:
* https://github.com/spotify/dockerfile-maven/issues/394
* https://github.com/spotify/dockerfile-maven/issues/367

The project has been marked as InActive and hence not accepting any new MRs.

> At this point, we're not developing or accepting new features or even fixing non-critical bugs. 

With the growing popularity of Mac M1 processor in at Workspaces, it breaks the build process and creates a lot of troubles for everyone in the team. Hence this project is trying to provide a solution for arm compatible builds.

**Note**: these packages are not going to be maintained on regular-basis, but we will be willing to accept contributions. They are also not tested very well so don't build for production systems.

Thanks for suggestion at https://github.com/spotify/dockerfile-maven/issues/367#issuecomment-1062624063 which helped me figure out how to build and provide the plugin for arm compatibility.

## How to Use

If you want to use the docker-client supporting `arm64` use `docker-client` package.

```xml
<dependency>
    <groupId>com.spotify</groupId>
    <artifactId>docker-client</artifactId>
    <version>8.16.1-SNAPSHOT</version>
</dependency>
```

If you just want to use the plugin, use the following:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>dockerfile-maven-plugin</artifactId>
            <version>1.4.14-SNAPSHOT</version>
        </plugin>
    </plugins>
</build>
```

## How do I fetch these images

This will be a little tricky to setup. Suggestions are welcome for a better approach.

Add github to your repository:

```xml
<repositories>
    <repository>
        <id>github</id>
        <url>https://maven.pkg.github.com/arm64-compat/dockerfile-maven-plugin</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </repository>
    <pluginRepository>
        <id>github</id>
        <url>https://maven.pkg.github.com/arm64-compat/dockerfile-maven-plugin</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </pluginRepository>
</repositories>
```

You may also use custom user `settings.xml` file to configure these repositories to include in your build system.

These packages are published on `github` and to download even a public package from github, it requires [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Thus you would need something like following in your settings.xml file:

```xml
<servers>
    <server>
        <id>github</id>
        <username>${GITHUB_USERNAME}</username>
        <password>${GITHUB_TOKEN}</password>
    </server>
</servers>  
```

You can then provide `$GITHUB_USERNAME` and `$GITHUB_TOKEN` using environment variables.

## Using package manager

The above can be a little tricky for maven newcomers to setup.

However much simpler approach would be if you are using your own package manager (like `nexus`) is to proxy this package to your package manager and then use from there directly like you do for your other dependencies.

Howevery the `-SNAPSHOT` version can remain, because the real-owner of the package is `spotify`. This is just provided for streamlining local builds.

Setting build profiles for `Arm64` based architectures would really help all the developers in the team to build without much hassel.

## License

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
