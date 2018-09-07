# Sourcemapper

Sourcemapper is a bit of golang to parse a sourcemap, as generated by webpack or similar, and spit out the original JavaScript files, recreating the source tree based on the file paths in the sourcemap.

## Usage

```None
:~$ ./sourcemapper
Usage of ./sourcemapper:
  -help
        Show help
  -output string
        Source file output directory
  -url string
        URL to the Sourcemap file
```

Sourcemapper will download the map file at `url`, and then spit the sources out into the directory defined by `output`

```None
doi@asov:~$ ./sourcemapper -o dhubsrc -u https://hub.docker.com/public/js/client.356c14916fb23f85707f.js.map
[+] Retriving Sourcemap from https://hub.docker.com/public/js/client.356c14916fb23f85707f.js.map
[+] Read 23045027 bytes, parsing JSON
[+] Retrieved Sourcemap with version 3, containing 1828 entries
[+] Writing 9076765 bytes to dhubsrc/webpack:/js/client.356c14916fb23f85707f.js
[+] Writing 1014 bytes to dhubsrc/webpack:/webpack/bootstrap 356c14916fb23f85707f
[+] Writing 3174 bytes to dhubsrc/webpack:/app/scripts/client.js
[+] Writing 281 bytes to dhubsrc/webpack:/~/babel-runtime/helpers/interop-require-default.js
[+] Writing 151 bytes to dhubsrc/webpack:/~/babel-core/polyfill.js
{snip}
[+] Writing 271 bytes to dhubsrc/webpack:/~/rc-tooltip/~/core-js/library/fn/object/set-prototype-of.js
[+] Writing 315 bytes to dhubsrc/webpack:/~/rc-tooltip/~/core-js/library/modules/es6.object.set-prototype-of.js
[+] Writing 1044 bytes to dhubsrc/webpack:/~/rc-tooltip/~/core-js/library/modules/_set-proto.js
[+] Writing 308 bytes to dhubsrc/webpack:/~/rc-tooltip/~/core-js/library/fn/object/create.js
[+] Writing 307 bytes to dhubsrc/webpack:/~/rc-tooltip/~/core-js/library/modules/es6.object.create.js
[+] Writing 360 bytes to dhubsrc/webpack:/~/rc-animate/~/core-js/library/fn/object/define-property.js
[+] Writing 371 bytes to dhubsrc/webpack:/~/rc-animate/~/core-js/library/modules/es6.object.define-property.js
[+] Writing 1041 bytes to dhubsrc/webpack:/~/rc-animate/~/babel-runtime/helpers/createClass.js
[+] done
doi@asov:~$ cd dhubsrc/
doi@asov:~/dhubsrc$ du -hs .
20M     .
doi@asov:~/dhubsrc$ cd webpack\:/
~/         app/       js/        webpack/   (webpack)/
doi@asov:~/dhubsrc$ cd webpack\:/app/scripts/
actions/     components/  middlewares/ reducers/    selectors/   stores/      vendor/
doi@asov:~/dhubsrc$ cd webpack\:/app/scripts/components/
```

## Limitations

Paths such as 'webpack:/~/src/whatever/omg.js' are pretty common, so this tool will likely fail on NTFS file systems. EXT4 works fine though :D