# Technical Diagrams

Architecture diagrams, flowcharts, and system overviews for docs and presentations.

## System Architecture

```
imgforge generate "A clean system architecture diagram on a white background. Three horizontal tiers labeled 'Client', 'API', and 'Storage'. Each tier has 2-3 rounded boxes connected by arrows. Use a blue and gray color scheme. Arrows are labeled with protocols like 'HTTPS', 'gRPC', 'SQL'. Professional, minimal, suitable for technical documentation." -s landscape -q high
```

## CI/CD Pipeline

```
imgforge generate "A horizontal pipeline diagram showing 5 stages: Build, Test, Lint, Deploy, Monitor. Each stage is a rounded rectangle with a small icon inside. Stages are connected by arrows. The 'Deploy' stage has a branch splitting to 'Staging' and 'Production'. Light background, flat design, muted blue and green palette." -s landscape -q high
```

## Database Schema

```
imgforge generate "An entity-relationship diagram showing 4 database tables connected by relationship lines. Tables have header rows in dark blue and field rows in white. Field names use monospace font. Relationships show 1:N and N:M cardinality markers. Clean white background, technical documentation style." -s landscape -q high
```

## API Flow

```
imgforge generate "A sequence diagram showing interaction between Client, Auth Server, and API. Three vertical lifelines with horizontal arrows between them labeled 'POST /login', '200 JWT', 'GET /data + Bearer token', '200 JSON response'. Clean black lines on white background, UML-style." -s portrait -q high
```
