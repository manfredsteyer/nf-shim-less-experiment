# Experiment: Using Native Federation without an ES-Polyfill in Prod-Mode

Prerequisites:
- Angular 17.1+
- Native Federation 17.1.6+

1. Start your solution in prod mode.
2. Open your browser and switch to the dev tools. 
3. Make the dev tools to display your app's markup and copy the following markup: ``<script type="importmap-shim">[...]</script>``
4. Open your `angular.json` and set ``skipHtmlTransform`` to true for your production build:

    ```json
    "architect": {
        "build": {
            "builder": "@angular-architects/native-federation:build",
            "options": {
            },
            "configurations": {
                "production": {
                    "target": "nf172:esbuild:production",
                    "skipHtmlTransform": true
                },
                "development": {
                    "target": "nf172:esbuild:development",
                    "dev": true
                }
            },
            "defaultConfiguration": "production"
        },
        [...]
    }
    ```

5. Temporarily change your ``main.ts`` so that it only imports your ``bootstrap.ts``:
   ```typescript
   import('./bootstrap');
   ```
6. Build your solution with ``ng build``.
7. Switch to your ``dist`` folder (`dist/<project-name>/browser`) and manually add the import map copied before into it. Make sure, it becomes the **first (!)** script tag of your page and **replace (!)** ``type="importmap-shim"`` by ``type="importmap"``.
8. Try out your solution, e.g., by using ``npx serve`` in the ``dist`` folder (`dist/<project-name>/browser`).
