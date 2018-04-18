参考资料: [react-app-rewired](https://github.com/timarney/react-app-rewired)  

rewireSass.js
```js
const { getLoader, loaderNameMatches } = require("react-app-rewired");
const path = require('path');

const rewireSass = function (config, env) {
    let sassExtension = [/\.scss$/, /\.sass$/];

    let fileLoader = getLoader(
        config.module.rules,
        rule => loaderNameMatches(rule, 'file-loader')
    );

    fileLoader.exclude.push(sassExtension);

    let cssRules = getLoader(
        config.module.rules,
        rule => String(rule.test) === String(/\.css$/)
    );

    let sassRules;

    if (env === "production") {
        sassRules = {
            test: sassExtension,
            loader: [
                ...cssRules.loader,
                { loader: "sass-loader" }
            ]
        };
    } else {
        sassRules = {
            test: sassExtension,
            use: [
                ...cssRules.use,
                { loader: "sass-loader" }
            ]
        };
    }

    const oneOfRule = config.module.rules.find((rule) => rule.oneOf !== undefined);
    if (oneOfRule) {
        oneOfRule.oneOf.unshift(sassRules);
    }
    else {
        config.module.rules.push(sassRules);
    }

    return config;
}

module.exports = {
    rewireSass
}
```
config-overrides.js
```
const {rewireSass} = require('./rewireSass')


module.exports = function override(config, env) {

  // do stuff with the webpack config...
  config = rewireSass(config, env);

  return config;
};
```
