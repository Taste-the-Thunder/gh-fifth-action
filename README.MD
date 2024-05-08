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