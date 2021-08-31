# Test 3rd party tools to install direct `peerDependencies` libraries
npm prior to v7, will not install plugin's `peerDependencies` automatically.
In order to properly maintain the plugin, like compile and publish, you will need a way to install these peerDeps in your dev env.
(This may not be an issue to [Monorepo](https://www.perforce.com/blog/vcs/what-monorepo))

npm v7 fixed the issue perfectly, so you don't need this topic anymore.  

## Instruction
Actually there are 2 ways to handle this issue:
1. Just add these `peerDependencies` to `devDependencies` to fit the dev env.  
Just these 'devDeps' are not real 'devDeps'.  

2. Use `prepare` script to let NPM to install these 'additional' deps via 3rd party tools, right after `npm install`, which is this topic talks about.  
```
"prepare": "npm-install-peers"
```
About `prepare`, please see NPM [life-cycle-scripts](https://docs.npmjs.com/cli/v7/using-npm/scripts#life-cycle-scripts).

## Test
**Test Design**  
[react-share](https://github.com/nygardk/react-share/blob/v4.4.0/package.json) has a peerDependencies of `react`  
[file-loader](https://github.com/webpack-contrib/file-loader/blob/v6.2.0/package.json) has a peerDependencies of `webpack`  
Use `react-share` as `peerDependencies` and `file-loader` as `dependencies`  

**Check Point**
1. Can install direct `peerDependencies`  
2. Can install `peerDependencies` of the `dependencies`  
3. Can install `peerDependencies` of the `peerDependencies`  
4. Can install `peerDependencies` of the `devDependencies`  
5. Install modifies `package.json`, `package-lock.json`  

## Tools
### npm-install-peers
![https://www.npmjs.com/package/npm-install-peers](https://img.shields.io/npm/v/npm-install-peers?style=flat-square)
![https://www.npmjs.com/package/npm-install-peers](https://img.shields.io/npm/dm/npm-install-peers?style=flat-square)

```
  "scripts": {
    "prepare": "npm-install-peers"
  },
```

### install-peers-cli
![https://www.npmjs.com/package/install-peers-cli](https://img.shields.io/npm/v/install-peers-cli?style=flat-square)
![https://www.npmjs.com/package/install-peers-cli](https://img.shields.io/npm/dm/install-peers-cli?style=flat-square)

```
  "scripts": {
    "prepare": "install-peers"
  },
```

### @team-griffin/install-self-peers
![https://www.npmjs.com/package/@team-griffin/install-self-peers](https://img.shields.io/npm/v/@team-griffin/install-self-peers?style=flat-square)
![https://www.npmjs.com/package/@team-griffin/install-self-peers](https://img.shields.io/npm/dm/@team-griffin/install-self-peers?style=flat-square)

```
  "scripts": {
    "prepare": "install-self-peers --npm -- --ignore-scripts"
  },
```

# Test Result
Run: `npm install`  

|   | npm-install-peers | install-peers-cli | @team-griffin/install-self-peers |
|:---:|:---:|:---:|:---:|
| Windows: | ✅<sup>(1)</sup> | ❌<sup>(2)</sup> | ❌<sup>(3)</sup> |
| macOS | ✅<sup>(1)</sup> | ✅<sup>(1)</sup> | ❌<sup>(3)</sup> |

Note:
1. All tools just only install direct `peerDependencies`, they don't install `peerDependencies` of the `devDependencies`, `peerDependencies` and `devDependencies`.  
1. Known issue, see https://github.com/alexindigo/install-peers-cli/issues/12
2. It adds the "peerDependencies" to "dependencies", thus package.json get updated, no good.

So, prefer to use `npm-install-peers`, and it is also the most popular one.  

## Disadvantage
Go with `prepare` script, `peerDependencies` will be re-installed every time you run `npm install`, which will slow down the `install` task.  
The best choice is to upgrade to npm v7. ^_^  
