LOD tools
=========

A collection of tools for Linked open Data Experiments, especially to be used in automated workflows (like in GitHub actions)

# `hdt-cpp`

This image includes `rdf2hdt`.

```
docker build -f docker/rdf2hdt/Dockerfile -t ghcr.io/cmahnke/lod-tools/rdf2hdt:latest .
```

## Usage

To convert a Turtle (`.ttl`) file to HDT format, mount the directory containing your input file into the container's `/data` volume:

```bash
docker run --rm -v $(pwd):/data ghcr.io/cmahnke/lod-tools/rdf2hdt:latest input.ttl output.hdt
```

For example, to convert a file called `ontology.ttl` located in your current working directory:

```bash
docker run --rm -v $(pwd):/data ghcr.io/cmahnke/lod-tools/rdf2hdt:latest ontology.ttl ontology.hdt
```

The resulting `ontology.hdt` file will be written to your current directory.

### Additional Options

Specify the base URI explicitly:

```bash
docker run --rm -v $(pwd):/data ghcr.io/cmahnke/lod-tools/rdf2hdt:latest -B "http://example.org/" input.ttl output.hdt
```

Use N-Triples instead of Turtle as input format:

```bash
docker run --rm -v $(pwd):/data ghcr.io/cmahnke/lod-tools/rdf2hdt:latest -f ntriples input.nt output.hdt
```

Show all available options:

```bash
docker run --rm ghcr.io/cmahnke/lod-tools/rdf2hdt:latest --help
```
