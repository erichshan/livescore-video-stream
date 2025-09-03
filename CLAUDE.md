# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a comprehensive analysis and technical specification repository for extending a live scoring golf service with real-time video streaming capabilities. The project focuses on solving synchronization challenges between score events and video streams in a Korean golf live scoring platform.

## Architecture and Design

### Core Challenge
- **Primary Problem**: Synchronizing score events (birdie notifications) with live video streams to prevent timing mismatches
- **Technical Requirement**: <500ms latency using WebRTC for real-time synchronization
- **Target Market**: Korean golf courses with existing cart-based scoring tablets

### System Architecture
The proposed system follows a hybrid architecture approach:

1. **Broadcasting Layer**: App/device → WebRTC Ingest → Media Server
2. **Distribution Layer**: 
   - WebRTC for small rooms (<100 viewers)
   - Low-Latency HLS for large rooms (>100 viewers)
3. **Synchronization Layer**: Score event server ↔ Synchronization engine ↔ Unified client

### Streaming Protocol Analysis
- **WebRTC**: 66-500ms latency, scales to ~300 users - **optimal for golf service**
- **Low-Latency HLS**: 2-5 seconds latency, thousands of users - **synchronization issues**
- **Traditional HLS**: 5-40 seconds latency - **unsuitable**

## Technology Stack Recommendations

### Phase 1: MVP (3 months)
- **Platform**: AWS IVS (50% discount for Korean region)
- **Target**: 4-30 person rooms
- **Features**: Mobile app streaming, basic synchronization
- **Tech Stack**: WebRTC for low latency, AWS infrastructure

### Phase 2: Scaling (6 months)
- **Hybrid Approach**: AWS IVS + LiveKit
- **Target**: 100-300 person rooms
- **Features**: Dedicated device support, advanced synchronization algorithms

### Phase 3: Optimization (12 months)
- **Self-hosted Solution**: LiveKit-based infrastructure
- **Target**: 1000+ person rooms
- **Features**: AI highlights, international expansion

## Cost Structure Analysis

### Service Tiers
- **라이트 (Light)**: 4-10 users, +₩30,000 monthly
- **스탠다드 (Standard)**: 30-100 users, +₩100,000 monthly  
- **프로 (Pro)**: 300-1000 users, +₩500,000 monthly

### Infrastructure Costs (Monthly)
- **AWS IVS** (with Korean discount): ₩30,000-500,000 per room size
- **Agora.io**: $11.97-$1,798.79 per room size
- **Self-hosted LiveKit**: Infrastructure costs only

## Technical Implementation Notes

### Synchronization Algorithm
```
tablet → score input (t1) → score server → client
video stream (t1-delay) → client
client applies delay compensation: video = t1 - measured_delay
```

### Infrastructure Requirements
- **Network**: 5Mbps upload, 10Mbps download (HD)
- **Server**: 8-core CPU, 16GB RAM, 500GB SSD (100 users)
- **CDN**: Korean IDC integration required

## Development Considerations

When working on this project:

1. **Focus on Synchronization**: The core value proposition depends on achieving <500ms latency
2. **Korean Market Specifics**: Leverage AWS Korean region discounts and local infrastructure
3. **Scalability Planning**: Design for the three-phase rollout (SaaS → Hybrid → Self-hosted)
4. **Cost Optimization**: Balance feature richness with operational costs at each tier
5. **Mobile-First**: Primary streaming source will be mobile apps, optimize accordingly

## Key Files and Structure

This repository contains a comprehensive analysis document (README.md) that serves as the complete technical specification and business plan. The document includes:

- Market analysis and competitive landscape
- Technical architecture diagrams (Mermaid format)
- Cost projections and pricing strategies
- Implementation roadmaps and timelines
- Infrastructure requirements and technology stack comparisons

## Strategic Decisions Made

1. **WebRTC First**: Prioritize WebRTC over HLS variants due to synchronization requirements
2. **AWS IVS MVP**: Start with AWS IVS for rapid deployment and Korean market advantages
3. **Hybrid Scaling**: Progress to hybrid architecture before full self-hosting
4. **Tiered Pricing**: Align streaming costs with existing chat service pricing tiers