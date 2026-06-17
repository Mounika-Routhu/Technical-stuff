## 1) What is `npm i`?

`npm i` is short for **`npm install`**.

### What it does
- Installs dependencies listed in **`package.json`**
- Creates **`node_modules`** folder
- Creates or updates **`package-lock.json`**

### Simple flow

```text
package.json  -->  npm install  -->  node_modules + package-lock.json
```

### Important point
- It can install the **latest allowed version** based on version symbols like `^` or `~`
- It may update the lock file

---

## 2) What is `npm ci`?

`npm ci` means **clean install**.

It is mainly used in **CI/CD pipelines**.

### What it does
- Installs dependencies using **exact versions** from **`package-lock.json`**
- Removes existing **`node_modules`** first
- Does **not** update the lock file
- Usually faster than `npm install`

### Simple flow

```text
package-lock.json  -->  npm ci  -->  exact same node_modules
```

---

## 3) `package.json` vs `package-lock.json`

### `package.json`
This is the **main project file** written/maintained by the developer.

It contains:
- project name and version
- scripts (`start`, `build`, `test`)
- direct dependencies
- allowed version range

### Example

```json
"dependencies": {
  "react": "^18.2.0"
}
```

### Meaning
- `^18.2.0` means npm can install compatible newer versions like `18.2.1`, `18.3.0`, etc. (but not `19.0.0`)

---

### `package-lock.json`
This is an **auto-generated lock file**.

It contains:
- exact installed version
- full dependency tree
- child dependency versions

### Example

```json
"react": "18.2.0"
```

### Meaning
- Install **only this exact version**

---

## 4) Easy way to remember

- **`package.json`** = what you **want**
- **`package-lock.json`** = what you **got**

---

## 5) Why is `npm ci` needed?

### Problem with `npm install`
If different people or pipelines run `npm install` at different times:
- slightly different versions may get installed
- app may work on one machine and fail on another
- builds can become inconsistent
- **also, most importantly in recent times, axios & tanstack are hacked, when pipelines run with npm i, malware version might get installed**

### Why `npm ci` helps
`npm ci` uses the lock file strictly, so:
- everyone gets the **same versions**
- pipeline build is reproducible
- fewer "works on my machine" issues
- faster for CI/CD

---

## 6) Real example

### In `package.json`

```json
"lodash": "^4.17.0"
```

This allows npm to install a compatible newer version.

### Suppose developer got

```text
4.17.21
```

That version gets stored in **`package-lock.json`**.

### Later in pipeline
- `npm install` may resolve versions again depending on lock/package state
- `npm ci` will install the **exact locked version** from the lock file

---

## 7) Quick comparison

### `npm install`
- used in local development
- reads `package.json`
- can update `package-lock.json`
- flexible versions
- slower than `npm ci`

### `npm ci`
- used in CI/CD
- requires `package-lock.json`
- installs exact versions only
- does not update lock file
- faster bcz it doesn't have to resolve version -> always clean install and more reliable

---

## 8) Visual summary

```text
              +-------------------+
              |   package.json    |
              |   (what you want) |
              +---------+---------+
                        |
                 npm install
                        |
                        v
              +-------------------+
              | package-lock.json |
              |  (exact snapshot) |
              +---------+---------+
                        |
                     npm ci
                        |
                        v
              +-------------------+
              |   node_modules    |
              |   exact versions  |
              +-------------------+
```

---

## 9) Best practice

### Use `npm install` when:
- adding a new package
- updating dependencies
- working locally in development

### Use `npm ci` when:
- running builds in CI/CD pipeline
- testing in clean environment
- you want exact same dependency setup every time

---

## 10) Final short summary

```text
npm i / npm install
- installs dependencies
- may update lock file
- flexible

npm ci
- clean install
- uses exact lock file versions
- no lock file changes
- best for CI/CD

package.json      = desired dependencies and scripts
package-lock.json = exact installed dependency snapshot
```
