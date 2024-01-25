---
title: Decentralized Pre-Trained Transformers (DPT) with Embedded Liquidity
description: Decentralized AI Market with Tokenized Models and Computational Resources
author: <a comma separated list of the author's or authors' name + GitHub username (in parenthesis), or name and email (in angle brackets).  Example, FirstName LastName (@GitHubUsername), FirstName LastName <foo@bar.com>, FirstName (@GitHubUsername) and GitHubUsername (@GitHubUsername)>
discussions-to: <URL>
status: Draft
type: Standards Track
category: ERC
created: 2024-01-25
requires: EIP 1671
---

## Abstract

<!--
  The Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.

  TODO: Remove this comment before submitting
-->

This Ethereum Improvement Proposal (EIP) introduces a framework for establishing a transparent and decentralized market
for AI services. The proposal suggests the tokenization of AI models as Non-Fungible Tokens (NFTs) named Decentralized
Pre-Trained Transformers (DPTs). Additionally, computational resources are tokenized as NFTs called Personality Pods,
which can launch Hives—helper smart contracts that invite DPT owners to utilize the resources provided. Each DPT has a
Bonding Curve helper contract attached, and the proposed framework aims to create a dynamic and liquid AI market.

## Motivation

<!--
  This section is optional.

  The motivation section should include a description of any nontrivial problems the EIP solves. It should not describe how the EIP solves those problems, unless it is not immediately obvious. It should not describe why the EIP should be made into a standard, unless it is not immediately obvious.

  With a few exceptions, external links are not allowed. If you feel that a particular resource would demonstrate a compelling case for your EIP, then save it as a printer-friendly PDF, put it in the assets folder, and link to that copy.

  TODO: Remove this comment before submitting
-->

The existing AI market lacks transparency and decentralization, hindering the efficient utilization of computational
resources and the fair trading of AI models. By introducing tokenization of AI models and computational resources on the
Ethereum blockchain, we aim to address the following issues:

1.  Lack of Transparency: Current AI markets often lack transparency in pricing, model performance, and resource
    utilization. Tokenization enables a transparent and immutable ledger of AI models and computational resources.

2.  Decentralization: The proposed framework leverages the decentralized nature of blockchain technology,
    eliminating the need for centralized intermediaries and providing a trustless environment for AI service providers
    and consumers.

3.  Liquidity: The introduction of a bonding curve contract for DPTs encourages liquidity by allowing users to
    seamlessly buy and sell AI models, contributing to a more dynamic and efficient market.

The existing AI market lacks transparency and decentralization, hindering the efficient utilization of computational
resources and the fair trading of AI models. By introducing tokenization of AI models and computational resources on the
Ethereum blockchain, we aim to address the following issues:

1.  Lack of Transparency: Current AI markets often lack transparency in pricing, model performance, and resource
    utilization. Tokenization enables a transparent and immutable ledger of AI models and computational resources.

2.  Decentralization: The proposed framework leverages the decentralized nature of blockchain technology,
    eliminating the need for centralized intermediaries and providing a trustless environment for AI service providers
    and consumers.

3.  Liquidity and Dynamic AI Models: The introduction of a bonding curve contract for DPTs, coupled with Personality
    Pods launching Hives, encourages liquidity by allowing users to seamlessly buy and sell AI models. Key holders
    interacting with AI models create key demand, particularly for interesting and successful models, resulting in
    backed liquidity that helps AI models evolve.s

## Specification

<!--
  The Specification section should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Ethereum platforms (besu, erigon, ethereumjs, go-ethereum, nethermind, or others).

  It is recommended to follow RFC 2119 and RFC 8170. Do not remove the key word definitions if RFC 2119 and RFC 8170 are followed.

  TODO: Remove this comment before submitting
-->

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

Specification

1. Decentralized Pre-Trained Transformers (DPTs):
    - DPTs are represented as NFTs on the Ethereum blockchain.
    - Each DPT contains metadata specifying the AI model details, performance metrics, and usage terms.
    - The associated bonding curve contract facilitates the trading of DPTs, ensuring price discovery and liquidity.

2. Tokenized Computational Resources - Personality Pods and Hives:
    - Computational resources are tokenized as NFTs called Personality Pods.
    - Personality Pods can launch Hives, which are helper contracts inviting DPT owners to utilize the associated
      computational resources.
    - DPTs can freely enter and leave Hives, providing flexibility to AI model owners.

3. Bonding Curve and Key Interaction:
    - Each DPT has a Bonding Curve helper contract attached.
    - Key holders, acquired through the bonding curve, are allowed to interact with the AI model, creating key demand.
    - Successful and interesting AI models can attract more key holders, further enhancing liquidity and model
      evolution.

4. Auxiliary Helper Smart Contracts:
    - Additional smart contracts may be introduced to handle governance, dispute resolution, and other aspects of the
      decentralized AI market.
    - Smart contracts should be designed to ensure fairness, security, and efficiency in the AI service ecosystem.

## Rationale

<!--
  The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

The proposed tokenization of AI models and computational resources leverages the unique features of the Ethereum
blockchain, including immutability, transparency, and decentralization. By establishing a standardized framework for
decentralized AI markets, we aim to foster innovation, collaboration, and efficiency within the AI community.

Community-Powered IP: Each DPT, with its unique digital identity and specialized functionality, becomes a
community-powered intellectual property.

Crowdsourcing: DPTs leverage crowdsourcing for creativity, governance, datasets, AI Models, and compute resources,
unlocking new business models and growth opportunities.

## Security Considerations

<!--
  All EIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life cycle of the proposal. For example, include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. EIP submissions missing the "Security Considerations" section will be rejected. An EIP cannot proceed to status "Final" without a Security Considerations discussion deemed sufficient by the reviewers.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

1.  Smart Contract Security: Implementations of smart contracts should undergo thorough security audits to ensure
    resistance against vulnerabilities and attacks.

2.  Privacy Concerns: The metadata associated with DPTs should consider privacy concerns, and appropriate measures
    should be taken to protect sensitive information.

3.  Governance and Dispute Resolution: Mechanisms for governance and dispute resolution in the decentralized AI market
    should be carefully designed to maintain fairness and prevent malicious activities.

4.  Compliance: The proposal should comply with relevant legal and regulatory frameworks to ensure the legitimacy of AI
    services traded on the platform.

This proposal is a draft, and further community feedback and collaboration are encouraged to refine and improve the
specifications outlined herein.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
