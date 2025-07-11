[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/augmented-nature-alphafold-mcp-server-badge.png)](https://mseep.ai/app/augmented-nature-alphafold-mcp-server)

![AlphaFold MCP Server Logo](alphafold-mcp-server-logo.png)
# AlphaFold MCP Server

A comprehensive Model Context Protocol (MCP) server that provides access to the AlphaFold Protein Structure Database through a rich set of tools and resources for protein structure prediction analysis.

## Overview

This MCP server enables seamless integration with AlphaFold's vast collection of protein structure predictions, offering tools for structure retrieval, confidence analysis, batch processing, and visualization preparation. Perfect for researchers, bioinformaticians, and structural biologists working with predicted protein structures.

## Features

### 🧬 Core Structure Tools

- **Structure Retrieval**: Get AlphaFold predictions by UniProt ID
- **Multi-format Downloads**: Support for PDB, CIF, BCIF, and JSON formats
- **Availability Checking**: Verify if predictions exist for specific proteins

### 🔍 Search & Discovery

- **Structure Search**: Find proteins by name, gene, or organism
- **Organism Browsing**: List all available structures for specific species
- **Coverage Statistics**: Get comprehensive organism-level statistics

### 📊 Confidence & Quality Analysis

- **Per-residue Confidence**: Detailed confidence scores for each amino acid
- **Region Analysis**: Identify high/low confidence structural regions
- **Quality Validation**: Assess overall prediction reliability

### ⚡ Batch Processing

- **Bulk Retrieval**: Process multiple proteins simultaneously
- **Batch Downloads**: Efficient multi-structure downloads
- **Parallel Analysis**: Confidence analysis for protein sets

### 🔬 Comparative Analysis

- **Structure Comparison**: Side-by-side analysis of multiple proteins
- **Similarity Search**: Find structurally related proteins
- **Coverage Comparison**: Analyze prediction completeness

### 🎨 Visualization Integration

- **PyMOL Scripts**: Ready-to-use visualization scripts
- **ChimeraX Integration**: Confidence-colored structure viewing
- **Custom Export Formats**: Flexible data export options

## Installation

```bash
# Clone or create the server directory
npm install

# Build the server
npm run build
```

## Usage

### As MCP Server

Add to your MCP configuration:

```json
{
  "mcpServers": {
    "alphafold-server": {
      "command": "node",
      "args": ["/path/to/alphafold-server/build/index.js"]
    }
  }
}
```

### Direct Usage

```bash
# Start the server
npm start

# Or run directly
node build/index.js
```

## Available Tools

### Core Structure Tools

#### `get_structure`

Retrieve AlphaFold structure prediction for a specific UniProt ID.

**Parameters:**

- `uniprotId` (required): UniProt accession (e.g., "P21359", "Q8N726")
- `format` (optional): Output format - "pdb", "cif", "bcif", or "json" (default: "json")

**Example:**

```javascript
{
  "uniprotId": "P04637",
  "format": "json"
}
```

#### `download_structure`

Download AlphaFold structure file in specified format.

**Parameters:**

- `uniprotId` (required): UniProt accession
- `format` (optional): File format - "pdb", "cif", or "bcif" (default: "pdb")

#### `check_availability`

Check if AlphaFold structure prediction is available for a UniProt ID.

**Parameters:**

- `uniprotId` (required): UniProt accession to check

### Search & Discovery Tools

#### `search_structures`

Search for available AlphaFold structures by protein name or gene.

**Parameters:**

- `query` (required): Search term (protein name, gene name, etc.)
- `organism` (optional): Filter by organism
- `size` (optional): Number of results (1-100, default: 25)

#### `list_by_organism`

List all available structures for a specific organism.

**Parameters:**

- `organism` (required): Organism name (e.g., "Homo sapiens", "Escherichia coli")
- `size` (optional): Number of results (1-100, default: 50)

#### `get_organism_stats`

Get statistics about AlphaFold coverage for an organism.

**Parameters:**

- `organism` (required): Organism name

### Confidence & Quality Tools

#### `get_confidence_scores`

Get per-residue confidence scores for a structure prediction.

**Parameters:**

- `uniprotId` (required): UniProt accession
- `threshold` (optional): Confidence threshold (0-100)

#### `analyze_confidence_regions`

Analyze confidence score distribution and identify high/low confidence regions.

**Parameters:**

- `uniprotId` (required): UniProt accession

#### `get_prediction_metadata`

Get metadata about the prediction including version, date, and quality metrics.

**Parameters:**

- `uniprotId` (required): UniProt accession

### Batch Processing Tools

#### `batch_structure_info`

Get structure information for multiple proteins simultaneously.

**Parameters:**

- `uniprotIds` (required): Array of UniProt accessions (max 50)
- `format` (optional): Output format - "json" or "summary" (default: "json")

#### `batch_download`

Download multiple structure files.

**Parameters:**

- `uniprotIds` (required): Array of UniProt accessions (max 20)
- `format` (optional): File format - "pdb" or "cif" (default: "pdb")

#### `batch_confidence_analysis`

Analyze confidence scores for multiple proteins.

**Parameters:**

- `uniprotIds` (required): Array of UniProt accessions (max 30)

### Comparative Analysis Tools

#### `compare_structures`

Compare multiple AlphaFold structures for analysis.

**Parameters:**

- `uniprotIds` (required): Array of UniProt accessions to compare (2-10)

#### `find_similar_structures`

Find AlphaFold structures similar to a given protein.

**Parameters:**

- `uniprotId` (required): Reference UniProt accession
- `organism` (optional): Filter by organism

### Coverage & Completeness Tools

#### `get_coverage_info`

Get information about sequence coverage in the AlphaFold prediction.

**Parameters:**

- `uniprotId` (required): UniProt accession

#### `validate_structure_quality`

Validate and assess the overall quality of an AlphaFold prediction.

**Parameters:**

- `uniprotId` (required): UniProt accession

### Export & Integration Tools

#### `export_for_pymol`

Export structure data formatted for PyMOL visualization.

**Parameters:**

- `uniprotId` (required): UniProt accession
- `includeConfidence` (optional): Include confidence score coloring (default: true)

#### `export_for_chimerax`

Export structure data formatted for ChimeraX visualization.

**Parameters:**

- `uniprotId` (required): UniProt accession
- `includeConfidence` (optional): Include confidence score coloring (default: true)

#### `get_api_status`

Check AlphaFold API status and database statistics.

**Parameters:** None

## Available Resources

### Resource Templates

#### `alphafold://structure/{uniprotId}`

**MIME Type:** `application/json`
**Description:** Complete AlphaFold structure prediction for a UniProt ID

#### `alphafold://pdb/{uniprotId}`

**MIME Type:** `chemical/x-pdb`
**Description:** PDB format structure file for a UniProt ID

#### `alphafold://confidence/{uniprotId}`

**MIME Type:** `application/json`
**Description:** Per-residue confidence scores for a structure prediction

#### `alphafold://summary/{organism}`

**MIME Type:** `application/json`
**Description:** Summary of all available structures for an organism

## Example Workflows

### Basic Structure Analysis

```javascript
// 1. Check if structure is available
await use_mcp_tool("alphafold-server", "check_availability", {
  uniprotId: "P04637",
});

// 2. Get structure metadata
await use_mcp_tool("alphafold-server", "get_prediction_metadata", {
  uniprotId: "P04637",
});

// 3. Analyze confidence scores
await use_mcp_tool("alphafold-server", "get_confidence_scores", {
  uniprotId: "P04637",
  threshold: 70,
});
```

### Comparative Study

```javascript
// Compare multiple related proteins
await use_mcp_tool("alphafold-server", "compare_structures", {
  uniprotIds: ["P04637", "P53350", "P63151"],
});

// Batch confidence analysis
await use_mcp_tool("alphafold-server", "batch_confidence_analysis", {
  uniprotIds: ["P04637", "P53350", "P63151"],
});
```

### Visualization Preparation

```javascript
// Export for PyMOL with confidence coloring
await use_mcp_tool("alphafold-server", "export_for_pymol", {
  uniprotId: "P04637",
  includeConfidence: true,
});

// Export for ChimeraX
await use_mcp_tool("alphafold-server", "export_for_chimerax", {
  uniprotId: "P04637",
  includeConfidence: true,
});
```

### Organism-wide Analysis

```javascript
// Get human protein statistics
await use_mcp_tool("alphafold-server", "get_organism_stats", {
  organism: "Homo sapiens",
});

// List available structures
await use_mcp_tool("alphafold-server", "list_by_organism", {
  organism: "Homo sapiens",
  size: 100,
});
```

## API Reference

The server connects to the AlphaFold API at `https://alphafold.ebi.ac.uk/api/` and provides structured access to:

- **Structure Predictions**: Complete protein structure data
- **Confidence Scores**: Per-residue reliability metrics
- **Metadata**: Prediction versions, dates, and quality information
- **Cross-references**: Links to other databases and resources

## Error Handling

The server includes comprehensive error handling for:

- Invalid UniProt IDs
- Missing structure predictions
- API connectivity issues
- Rate limiting and timeouts
- Malformed requests

## Rate Limiting

Please be mindful of API usage:

- Batch operations are limited to reasonable sizes
- Large requests are automatically chunked
- Built-in delays prevent API overload

## Contributing

Contributions are welcome! Please ensure:

- TypeScript type safety
- Comprehensive error handling
- Documentation for new features
- Testing with real AlphaFold data

## About

Developed by **Augmented Nature** - [augmentednature.ai](https://augmentednature.ai)

Augmented Nature specializes in creating AI-powered tools and infrastructure for scientific research and data analysis.

## License

MIT License - see LICENSE file for details.

## Citation

If you use this MCP server in your research, please cite:

- AlphaFold Database: https://alphafold.ebi.ac.uk/
- Model Context Protocol: https://modelcontextprotocol.io/

## Support

For issues and questions:

- Check the AlphaFold API documentation
- Review error messages for debugging hints
- Ensure UniProt IDs are valid and current

---

**Note:** This server provides a convenient interface to AlphaFold data but does not store or cache structure data. All data is retrieved directly from the official AlphaFold API.

## Citation
If you use this project in your research or publications, please cite it as follows:

```bibtex @misc{alphafoldmcp2025, 
author = {Moudather Chelbi},
title = {AlphaFold MCP Server},
year = {2025},
howpublished = {https://github.com/Augmented-Nature/AlphaFold-MCP-Server},
note = {Accessed: 2025-06-29}
