# How To Github Packages (NPM)

Gå till dina [Personal access tokens](https://github.com/settings/tokens) settings på GitHub
-> Developer settings -> Personal access tokens

Generera en ny token "Generate personal access token"

Välj åtminstone följande scope:
- write:packages
- delete:packages

När du väl har din token så kan du authentisera dig på två sätt.
Authentiseringen ger dig tillgång till de privata paket repositories
som din användare har tillgång till.

Du kan använda din datoranvändares `~/.npmrc` fil i hemmakatalogen, med följande:
```
//npm.pkg.github.com/:_authToken=TOKEN
```

Du kan också logga in med hjälp av NPM. Kör då dessa kommandon i terminalen:
```sh
$ npm login --registry=https://npm.pkg.github.com
> Username: USERNAME
> Password: TOKEN
> Email: PUBLIC-EMAIL-ADDRESS
```
Nu kan du interagera med Githubs packet-system (NPM)

## Skapa ett eget paket

Om du har flera paket som du vill publicera från varje repository.
Modifiera `package.json` filen i under-katalogen med `directory` tillägget under `repository`.

```json
"repository": {
  "type": "git",
  "url": "git+https://github.com/OWNER/REPOSITORY.git",
  "directory": "packages/name"
}
```

Själva paketet kommer behöva vara `scoped` vilket innebär att det har en `owner` innan.
```
@owner/repository
```

Nu för att ingen av misstag ska lyckas publicera biblioteket till npm.org, så behövs
en `.npmrc` i samma mapp som din `package.json` fil. filen behöver innehålla följande:
```
registry=https://npm.pkg.github.com/OWNER
```

En ytterliggare försäkringsåtgärd kan vara att addera paket registreringen i `package.json` också.

```json
"publishConfig": {
  "registry":"https://npm.pkg.github.com"
}
```

Du behöver också verifiera att paketnamnet i `package.json` har scopet med `@owner/package`.

För att sedan faktiskt publicera paketet, kör:
```sh
$ npm publish
```

## Konsumera det skapade paketet

På samma sätt som ovan, authentisera dig mot Github. Om det gäller en deploy server, så behöver det ske vid bygge och kan då skilja sig.

För att vi ska kunna konsumera paket från Github, så skapa en ny fil `.npmrc` i samma mapp som din `package.json` fil. filen behöver innehålla följande:
```
registry=https://npm.pkg.github.com/OWNER
```

Sedan är det bara börja använda paketen. Du behöver addera `scopet` för organisationen `OWNER`.
```
npm install --save @owner/repository
```

Du ska som vanligt kunna komma åt de paket som finns på [NPMjs](https://npmjs.com).
