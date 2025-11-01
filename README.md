# Tranport Propeties Solver

A Python tool for calculating effective thermal conductivity of rocks using various mixing models.

## Features

- **Multiple Mixing Models**: LIH, WIU, WIL, WIA, HSU, HSL, HSA, GSA, MAX, BRG
- **Hierarchical Processing**: Support for multiple hierarchy levels
- **Rock Type Support**: Carbonates, Sandstones
- **Flexible Input**: Easy-to-create input files

## Quick Start

### Installation

```bash
!git clone https://github.com/MariiaKalinina/RPM.git
pip install -r requirements.txt
%cd RPM/src
!python forward_isotropic_solver.py ../examples/Input-1.txt
