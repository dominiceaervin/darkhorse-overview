# DarkHorse

Real-time algorithmic trading system for ES futures, built in Python and deployed on Ubuntu Linux via systemd. Handles live market data streams, broker API integration, and automated risk controls.

Built through daily pair-programming with Claude Code.

> **Status:** Live production system. Trades a funded Tradovate futures account with automated risk controls (balance kill switch, health-check watchdogs, orphan-order reconciliation). Source code is private due to live-account credentials and strategy specifics.

## Architecture

**Signal detection**
- Real-time 1-minute bar processing via Databento WebSocket feeds
- Failed-breakdown pattern detection on trader-provided support/resistance levels
- Multi-timeframe stab-reversion analysis on 15-min and 5-min bars

**Execution**
- Tradovate REST + WebSocket integration for order placement and fill reconciliation
- OSO (One-Sends-Other) bracket orders with GTC time-in-force
- Automatic front-month contract rollover via volume-vote resolver

**Risk management**
- Multi-layer failure detection: in-process feed watchdog, cron-based liveness check, systemd auto-restart
- Automated balance kill switch (systemctl stop on drawdown breach)
- Orphan-order and naked-position reconciler running every 15 seconds
- Bracket registration grace window to survive broker API propagation lag

**Observability**
- Telegram bot integration for real-time trade alerts + operator commands
- Systemd journal-based logging with grep-friendly structured events
- Streamlit dashboard for account state, position tracking, and trade journal
- Multi-environment deployment (live + demo) across DigitalOcean droplets

**Research pipeline**
- Statistical validation via LightGBM ensembles, Random Forest, and MLP
- Chronological walk-forward validation (3-fold, non-overlapping test windows)
- Block-permutation Monte Carlo for tail-risk quantification and kill-switch calibration
- Backtest replay with slippage, commission, and cap-limit fidelity

**Deployment**
- Systemd unit files with Restart=always and memory limits
- Rsync-based deploy pipeline with per-environment configuration
- Cron watchdogs at 5-minute intervals (liveness, balance, contract-rollover)
- SSH-based remote administration

## Tech Stack

Python 3.12 • pandas • NumPy • LightGBM • scikit-learn • Databento • Tradovate REST/WS • systemd • cron • DigitalOcean • Streamlit • Telegram Bot API

## Status

In active development. Source code is private — this repo documents architecture and design decisions. A subset of the statistical validation methodology (walk-forward + Monte Carlo + ensemble ML) is extracted as a standalone library at [darkhorse-ml-validation](https://github.com/dominiceaervin/darkhorse-ml-validation).

## Contact

Dominic Ervin — dominiceaervin@gmail.com
