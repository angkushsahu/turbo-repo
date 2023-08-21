# Turbo Repo configuration for Nest.js and React.js

1. create a folder named project_name

   ```bash
   mkdir project_name
   ```

2. Change directory to project_name

   ```bash
   cd project_name
   ```

3. Install turbo as dev-dependency

   ```bash
   npm i -D turbo
   ```

4. Create a folder named apps and change directory to it

   ```bash
   mkdir apps && cd apps
   ```

5. create a server folder
   for nest : `nest new server`

6. create a client folder
   for react: `npm create vite@latest` (then select options as per need)

7. update root package.json as follows
   package.json should be like this:

   ```json
   {
      "devDependencies": {
         "turbo": "^1.10.9"
      },
      "scripts": {
         "dev": "turbo run dev",
         "build": "turbo run build",
         "start": "node apps/server/dist/main"
      },
      "workspaces": ["apps/*"]
   }
   ```

8. create a file called turbo.json in the root directory

   ```json
   {
      "$schema": "https://turborepo.org/schema.json",
      "pipeline": {
         "dev": {
            "cache": false
         },
         "build": {
            "dependsOn": ["^build"],
            "outputs": ["dist/**"]
         }
      }
   }
   ```

9. Make sure both client and server directories have a npm command `npm run dev` that spins up a local development server, **(any other command will result into error)**. Also, both the directories, server and client, should have another npm command `npm run build` which builds the respective projects for production.

   > Notes:
   >
   > > If you have a nest js project, than you manually have to update the `start:dev` command to `dev` command in package.json inside server directory
   > > OR
   > > Add the script `"dev": "nest start --watch --preserveWatchOutput"`

## Nest.js serve static files for production

Install serve static dependency for nest.js

From the root directory, run the following command

```bash
npm install --workspace server @nestjs/serve-static
```

Inside

```js
import { ServeStaticModule } from "@nestjs/serve-static";
import { join } from "path";

@Module({
   imports: [
      ServeStaticModule.forRoot({
         rootPath: join(__dirname, "../..", "client", "dist"),
      }),
   ],
})
export class AppModule {}
```

## Extra tip for coming so far

to install dependencies in server and client, we don't have to keep on changing directory from client to server and vice versa. Instead, we can be on the root directory and install using the following command:

```bash
npm install --workspace folder_name dependency_name
```

For Example, say we want to install react-router-dom inside the client folder, we can use the following command

```bash
npm install --workspace client react-router-dom
```
