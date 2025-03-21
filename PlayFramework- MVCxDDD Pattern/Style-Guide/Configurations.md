[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

---
# Configurations

Follow this short guide on how we should configure our codebase.

## application.conf

Never put sensitive datas here.

```scala
slick.dbs.default {
  profile = "slick.jdbc.PostgresProfile$"
  db {
    driver = "org.postgresql.Driver"
    url = ${?DB_URL}
    user = ${?DB_USER}
    password = ${?DB_PASSWORD}
    keepAliveConnection = true
  }
}
```

Never ever edit the `application.conf` especially the dynamic values.
One of the most common things to help you in development are these values:

```scala
"slick.dbs.default.db.profile" -> "slick.jdbc.PostgresProfile",
"slick.dbs.default.db.driver"  -> "org.postgresql.Driver",
"slick.dbs.default.db.url"     -> "jdbc:postgresql://localhost:5432/checkme",
"slick.dbs.default.db.keepAliveConnection"           -> "true",
"play.server.websocket.periodic-keep-alive-max-idle" -> "10 seconds",
"play.server.websocket.periodic-keep-alive-mode"     -> "pong"
```

You can just put it in the `build.sbt` so that you don't have to touch `application.conf`

```scala
// build.sbt
PlayKeys.devSettings ++= Seq(
  "slick.dbs.default.db.profile" -> "slick.jdbc.PostgresProfile",
  "slick.dbs.default.db.driver"  -> "org.postgresql.Driver",
  "slick.dbs.default.db.url"     -> "jdbc:postgresql://localhost:5432/checkme",
  "slick.dbs.default.db.keepAliveConnection"           -> "true",
  "play.server.websocket.periodic-keep-alive-max-idle" -> "10 seconds",
  "play.server.websocket.periodic-keep-alive-mode"     -> "pong"
)
```

## build.sbt

### Versioning

Update the version based on version this is made every end of the sprint.

```scala
version := "1.0.1"
```

### Adding Dependency

Always make sure that string dependency will be last and the non-strings to be first.

```scala
libraryDependencies ++= Seq(
  guice,
  specs2                    % Test,
  "org.scalatestplus.play" %% "scalatestplus-play"    % "7.0.1" % Test,
  "org.playframework"      %% "play-slick"            % "6.1.1",
  "org.playframework"      %% "play-slick-evolutions" % "6.1.1",
  "org.postgresql"          % "postgresql"            % "42.7.4",
  "org.mindrot"             % "jbcrypt"               % "0.4",
  "org.apache.pekko"       %% "pekko-actor"           % "1.0.3",
  "org.typelevel"          %% "cats-core"             % "2.12.0"
)
```

---

[Previous: Routes](Style-Guide/routes)