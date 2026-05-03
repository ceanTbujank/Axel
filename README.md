## prtbuff-wrapper

unofficial api wrapper for prtbuff extensions (reverse engineered)

### warning

⚠️ this is NOT official. may break at any time. use at your own risk.

### install

```javascript
npm install prtbuff-wrapper
```

### basic usage

```javascript
const prtbuff = require('prtbuff-wrapper')

const client = new prtbuff({
  apiKey: process.env.prtbuff_API_KEY
})

// search extensions
const results = await client.search('github')
console.log(results)

// execute command
await client.executeCommand('github.searchRepositories', {
  query: 'nodejs'
})
```

### available methods

```javascript
// extensions
client.listExtensions()
client.installExtension(id)
client.uninstallExtension(id)

// commands
client.listCommands(extensionId)
client.executeCommand(commandId, params)

// snippets
client.getSnippets()
client.createSnippet({ name, text })

// hotkeys
client.getHotkeys()
client.setHotkey(commandId, keys)
```

### authentication

get api key from:
1. open prtbuff
2. preferences > advanced
3. copy api token

set in env:

```bash
export prtbuff_API_KEY=your_token_here
```

### protocol

communicates via **ipc-bridge** (inter-process) and **msgpack-rpc** serialization

uses **prtbuff-protocol-v2** message format ([reverse-engineered spec](https://github.com/prtbuff-wrapper/protocol-docs))

### limitations

- read-only for some commands
- no access to local file browser
- clipboard commands limited
- requires prtbuff pro for some features

### examples

#### auto-install extensions

```javascript
const extensions = ['github', 'jira', 'linear']

for (const ext of extensions) {
  await client.installExtension(ext)
  console.log(`Installed ${ext}`)
}
```

#### backup snippets

```javascript
const snippets = await client.getSnippets()
fs.writeFileSync('snippets-backup.json', JSON.stringify(snippets))
```

#### trigger command from cli

```bash
node -e "
const prtbuff = require('prtbuff-wrapper');
const client = new prtbuff();
client.executeCommand('system.toggleDoNotDisturb');
"
```

### troubleshooting

**authentication fails?**  
make sure prtbuff is running and api key is valid

**command not found?**  
check extension is installed: `client.listExtensions()`

**permission denied?**  
some commands require accessibility permissions in system preferences

### alternatives

- **prtbuff official api** (paid, limited)
- **applescript** (slow, unreliable)
- **keyboard maestro** (not programmatic)

MIT • not affiliated with prtbuff
