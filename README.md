# TD_Obsidian
Access to Obsidian from TouchDesigner

# Install

1. Install plugin "local rest api" for Obsidian
2. Put API KEY from plugin config screen in TD API_KEY field

# Actions:
- GetNotesList() - return notes list
- OpenNote() - open note in obsidian
- GetNoteContent() - retrieve note src


# TD_Obsidian

A TouchDesigner extension for integrating with Obsidian knowledge management through the Obsidian Local REST API.

## Overview

TD_Obsidian enables seamless interaction between TouchDesigner and your Obsidian vault, allowing you to:
- Open and create notes in Obsidian from TouchDesigner
- Retrieve note content for processing or display
- List all notes in your vault
- Randomly access notes for creative workflows

## Features

### Core API Functions
- **OpenNote** - Opens or creates notes in Obsidian
- **GetNoteContent** - Retrieves markdown content from notes
- **GetNotesList** - Lists all files in your Obsidian vault
- **Unicode Support** - Handles non-ASCII characters (Cyrillic, etc.)

### TouchDesigner Integration
- Custom parameter pages for easy control
- Pulse parameters for triggering operations
- Automatic content storage in parameters and DATs
- Error handling and debug output
- SSL warning suppression for localhost connections

## Installation

### Prerequisites
1. **Obsidian** with the [Local REST API plugin](https://github.com/coddingtonbear/obsidian-local-rest-api) installed
2. **TouchDesigner** 2023.11600 or later

### Setup
1. Copy the `TD_ObsidianEXT.py` file to your TouchDesigner project
2. Configure the Local REST API plugin in Obsidian
3. Set your API key in the extension parameters

## Usage

### Parameters

#### Controls Page
- **Startsearch** - Search functionality trigger
- **Clearnoteslist** - Clear the notes list display  
- **Getnoteslist** - Refresh the list of vault files

#### Obsidian Page
- **Filename** (string) - Target note filename (e.g., "my-note.md")
- **Opennote** (pulse) - Opens/creates the specified note
- **Getnotecontent** (pulse) - Retrieves content from the note
- **Getnoteslist** (pulse) - Lists all vault files
- **Clearnodelist** (pulse) - Clears the notes display
- **Content** (read-only) - Displays retrieved note content

### Basic Workflow
1. Set the **Filename** parameter to your desired note
2. Click **Opennote** to open/create the note in Obsidian
3. Click **Getnotecontent** to retrieve and display the content
4. Use **Getnoteslist** to see all available notes

### Random Note Selection
Use the included chopexec callback to randomly select and open notes:
```python
from random import randint

def onOffToOn(channel, sampleIndex, val, prev):
    note_num = randint(0, op('notes').numRows-1)
    op.obsidian.par.Filename = op('notes')[note_num, 0]
    op.obsidian.par.Opennote.pulse()
    op.obsidian.par.Getnotecontent.pulse()
```

## API Endpoints

The extension uses the following Obsidian Local REST API endpoints:

- `POST https://127.0.0.1:27124/open/{filename}` - Open/create note
- `GET https://127.0.0.1:27124/{filename}` - Get note content
- `GET https://127.0.0.1:27124/vault/` - List vault files

## Configuration

### Obsidian Local REST API
1. Install the Local REST API plugin in Obsidian
2. Configure the plugin settings:
   - Enable HTTPS (recommended)
   - Set port to 27124 (default)
   - Generate an API key
3. Copy the API key to the TouchDesigner extension

### TouchDesigner Extension
- Set the `ApiKey` parameter with your Obsidian API key
- Ensure SSL warnings are disabled for localhost (handled automatically)

## File Structure

```
TD_ObsidianSrc/
├── README.md
├── Project.toe
├── td.version
├── Modules/
│   └── suspects/
│       └── project/
│           ├── TD_ObsidianEXT.py    # Main extension
│           ├── TD_Obsidian.tox      # TouchDesigner component
│           └── configs.tox          # Configuration component
├── Configs/
│   ├── General/
│   └── Local/
└── Data/
    ├── eventEmitter_EventDefinition.json
    ├── Init_Segments.tsv
    └── opStore_Storetable.tsv
```

## Error Handling

The extension includes comprehensive error handling:
- URL encoding for non-ASCII filenames
- SSL certificate verification bypass for localhost
- Debug output for troubleshooting
- Graceful fallbacks for missing files or network issues

## Debug Output

When operations are performed, you'll see detailed console output:
```
OpenNote: Original filename: 'my-note.md' (length: 10)
OpenNote: Encoded filename: 'my-note.md'
OpenNote: Full URL: 'https://127.0.0.1:27124/open/my-note.md'
OpenNote success: Opened/created 'my-note.md'
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with TouchDesigner
5. Submit a pull request

## License

This project is open source. Please check the license file for details.

## Support

For issues and questions:
- GitHub Issues: [TD_Obsidian Issues](https://github.com/alexeyroudenko/TD_Obsidian/issues)
- Author: [arthew0.online](https://arthew0.online/)

## Version History

- **v0.5** - Added Obsidian Local REST API integration
- **v0.4** - Implemented custom parameters and pulse controls
- **v0.3** - Added Unicode and URL encoding support
- **v0.2** - Basic note operations
- **v0.1** - Initial extension framework