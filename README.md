# DPI (Deep Packet Inspection) System

A high-performance C++ Deep Packet Inspection (DPI) engine that analyzes PCAP traffic, identifies application-level protocols (via SNI/HTTP), and applies filtering rules.

## Features
- Parses packets from L2–L7 (Ethernet → TCP/UDP → Application)
- Extracts SNI from TLS and Host from HTTP
- Classifies traffic (YouTube, Facebook, etc.)
- Flow-based blocking (IP, domain, app)
- Multi-threaded pipeline for scalable processing
- Generates filtered PCAP output + traffic stats

## Tech Stack
- C++17
- Multi-threading (mutex, condition variables)
- Custom PCAP parsing

## Project Structure
include/        # Headers (parser, extractor, rules, threading)
src/            # Core implementation
  ├── main_working.cpp   # Single-threaded version
  └── dpi_mt.cpp         # Multi-threaded version
test_dpi.pcap  # Sample input

## Build

Single-threaded:
g++ -std=c++17 -O2 -I include -o dpi_simple src/main_working.cpp src/*.cpp

Multi-threaded:
g++ -std=c++17 -pthread -O2 -I include -o dpi_engine src/dpi_mt.cpp src/*.cpp

## Run
./dpi_engine input.pcap output.pcap

With rules:
./dpi_engine input.pcap output.pcap \
  --block-app YouTube \
  --block-domain facebook \
  --block-ip 192.168.1.50

## How It Works
1. Reads packets from PCAP
2. Parses headers (Ethernet, IP, TCP/UDP)
3. Extracts SNI/Host for app detection
4. Tracks flows using 5-tuple
5. Applies blocking rules
6. Writes filtered packets + stats

## Use Cases
- Network monitoring
- Traffic filtering / firewall systems
- Cybersecurity research
- ISP-level traffic analysis
