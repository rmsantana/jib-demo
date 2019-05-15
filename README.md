# Jib-Demo

[![build status](https://gitlab.com/rafael.santana/jib-demo/badges/master/build.svg)](https://gitlab.com/rafael.santana/jib-demo/commits/master)

This project is a demonstration of how your Gitlab-CI can push your docker images to Gitlab Registry using [Google Jib](https://github.com/GoogleContainerTools/jib) and Maven.

#### The POM file

To push the images to the Gitlab Registry, we need to authenticate on the registry, so we pass the data inside the element `<auth>`, passing the `<username>` as `gitlab-ci-token` and the `<password>` as the token retrive from the CI job.

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
		<plugin>
			<groupId>com.google.cloud.tools</groupId>
			<artifactId>jib-maven-plugin</artifactId>
			<version>1.1.2</version>
			<configuration>
				<to>
					<image>${env.CI_REGISTRY}/${env.CI_PROJECT_PATH}</image>
					<auth>
						<username>gitlab-ci-token</username>
						<password>${env.CI_JOB_TOKEN}</password>
					</auth>
				</to>
			</configuration>
		</plugin>
	</plugins>
</build>
```

It's possible to also add multiple tags to the image. The element `<to>` has a element `<tags>`, wich is a list of `<tag>`, so you can have as many tags as you like, the way you like.

```xml
<to>
    ...
    <tags>
    	<tag>${project.version}</tag>
    	<tag>latest</tag>
    </tags>
    ...
</to>
```
