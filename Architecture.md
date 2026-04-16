# SynapseMesh Architecture

SynapseMesh is a stateless middleware service designed for high-concurrency AI agent environments.

## Core Components
- **The Orchestrator:** Written in Go for low-latency gRPC handling.
- **Consistency Manager:** Implements the "Quorum" logic to decide when a write is considered "committed."
- **CRDT Engine:** Uses LWW (Last-Write-Wins) Element Sets to merge concurrent agent inputs without database locks.

## Data Flow
1. **Agent Request:** Agent sends a memory update via gRPC.
2. **Auth & Scope Check:** Middleware validates if the Agent has `WRITE` permissions for that specific memory scope.
3. **Conflict Resolution:** The CRDT engine compares the incoming `Vector Clock` with the existing state.
4. **Persistence:** The resolved state is mirrored to Redis (for speed) and a Vector DB (for long-term semantic retrieval).



## Deployment
SynapseMesh is designed to be deployed as a Dockerized sidecar or a standalone Kubernetes service, scaling horizontally as the number of agents increases.