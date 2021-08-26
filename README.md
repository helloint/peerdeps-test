# Test 3rd Party Tools to install direct `peerDependencies`

## npm-install-peers
![https://www.npmjs.com/package/npm-install-peers](https://img.shields.io/npm/v/npm-install-peers?style=flat-square)
![https://www.npmjs.com/package/npm-install-peers](https://img.shields.io/npm/dm/npm-install-peers?style=flat-square)

```
  "scripts": {
    "prepare": "npm-install-peers"
  },
```

## install-peers-cli
![https://www.npmjs.com/package/install-peers-cli](https://img.shields.io/npm/v/install-peers-cli?style=flat-square)
![https://www.npmjs.com/package/install-peers-cli](https://img.shields.io/npm/dm/install-peers-cli?style=flat-square)

```
  "scripts": {
    "prepare": "install-peers"
  },
```

## @team-griffin/install-self-peers
![https://www.npmjs.com/package/@team-griffin/install-self-peers](https://img.shields.io/npm/v/@team-griffin/install-self-peers?style=flat-square)
![https://www.npmjs.com/package/@team-griffin/install-self-peers](https://img.shields.io/npm/dm/@team-griffin/install-self-peers?style=flat-square)

```
  "scripts": {
    "prepare": "install-self-peers --npm -- --ignore-scripts"
  },
```

# Instruction
When run `npm install` to prepare the environment, we want the `peerDependencies` to be installed as well.
The best way is to setup `prepare` script as it will be triggered right after `npm install`. See NPM [life-cycle-scripts](https://docs.npmjs.com/cli/v7/using-npm/scripts#life-cycle-scripts).
  
We also hope this install won't affect package.json as well as package-lock.json, means all the affects are just happened in local.

# Test Result
Test `npm install`  

|   | npm-install-peers | install-peers-cli | @team-griffin/install-self-peers |
|:---:|:---:|:---:|:---:|
| Windows: | ✅ | ❌<sup>(1)</sup> | ❌<sup>(2)</sup> |
| macOS | ✅ | ✅ | ❌<sup>(2)</sup> |

Note:
1. Known issue, see https://github.com/alexindigo/install-peers-cli/issues/12
2. It adds the "peerDependencies" to "dependencies", thus package.json get updated, no good.

So, prefer to use `npm-install-peers`, and it is also the most popular one.  

## Disadvantage
`peerDependencies` will be re-installed every time you run `npm install`, which will slow down the regular works.
So the hope is NPM can take over the works in future.  

Ups to you ^_^
