## Overview

Guardrail is a smart contract security layer designed to protect web3 protocols from exploits.  
It provides real-time monitoring, configurable security policies, and a mix of off-chain and on-chain techniques to detect economic risks, anomalies, and vulnerabilities in smart contracts.  
The solution aims to stop advanced attacks by monitoring and preventing risky transactions. Guardrail combines protocol expertise with its security research to create tailored policies for projects.

### Example exploits guardrails aims to protect include:

- **Re-entrancy Attacks**: Exploiting vulnerabilities in smart contract logic that allow repeated calls to withdraw funds before the contract updates its state.
- **Flash Loan Attacks**: Flash loans allow users to borrow assets without collateral, provided they repay within the same transaction. Attackers exploit flash loans to manipulate markets, execute complex arbitrage, or trigger vulnerabilities in other contracts.  
  TL/DR: A malicious actor takes out a flash loan, uses it to manipulate the price of some other market, repays the loan, and keeps the profit for themselves.
- **Oracle Manipulation**: Smart contracts often rely on oracles for external data, like asset prices. An attacker can manipulate or provide false data to the oracle, leading the contract to make incorrect calculations or actions.

## Defining Features

- **Protect smart contracts in real time**
- **Two are better than one** - Guardrail provides security research, and the clients know their protocol. The best policies result from the combination of both.
- **Easy to use** - low false positives
- **Product is modular** - Start with guards/simulation, move to prevention and on-chain verification

## Guards

- **Monitor**  
  Run security policies in real time or in simulation to get potential violations.
- **Prevent**  
  Enforce policies on-chain in real time by integrating Guardrail into contracts.
