## Notes

Learn to Upload Artifects
EX Single folder:
```
- name: Upload Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: dist-files
          path: dist
```

EX multiple folder
```
- name: Upload Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: dist-files
          path: |
            dist
            package.json
```

### To Download Artifacts

Ex:
```
- name: Get Build Artifects
        uses: actions/download-artifact@v4
        with:
          name: dist-files
      - name: Output content
        run: ls
```

### Globle Variable setup

EX:
```
  build:
    needs: test
    runs-on: ubuntu-latest
    outputs:
      script-file: ${{ steps.publish.outputs.script-file }}
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Build website
        run: npm run build
      - name: Publish JS files
        id: publish
        run: find dist/assets/*js -type f -execdir echo 'script-file={}' >> $GITHUB_OUTPUT ';'
      - name: Upload Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: dist-files
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Get Build Artifects
        uses: actions/download-artifact@v4
        with:
          name: dist-files
      - name: Output content
        run: ls
      - name: Output filename
        run: echo '${{ needs.build.outputs.script-file }}'
      - name: Deploy
        run: echo "Deploying..."
```


on the `build` jobs you can see  
``` 
    outputs:
      script-file: ${{ steps.publish.outputs.script-file }}
```

`script-file` is variable name that define under, `steps` that isgithub action variable, `publish` is the id-key that also define under, `outputs` is again github variable, `script-file` is again user define

```
    name: Publish JS files
        id: publish
        run: find dist/assets/*js -type f -execdir echo 'script-file={}' >> $GITHUB_OUTPUT ';'
```

`id: publish` is id:key define, it's name is up to user, `find dist/assets/*js -type f -execdir echo 'script-file={}' >> $GITHUB_OUTPUT ';'` use to fetch the file name, `script-file={}` the first is variable and other thing is use to fetch, that can be anything like string or array, `>> $GITHUB_OUTPUT` in that pass the file name to globle output variable.


### Cache

In this project it is use to added on node_module 
```
    - name: cache Dependancy
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: desp-node-modules-${{ hashFiles('**/package-lock.json') }}
```

user `actions/cache@v3` we can cache the nodules, `path: ~/.npm` is use for if you have any command fire useing npm that will auto added on cache, `key: desp-node-modules-${{ hashFiles('**/package-lock.json') }}` in this dynamic file name created using `haseFile()` so if anytime package updated that time only it generate new cache