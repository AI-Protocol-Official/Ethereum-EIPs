---
title: Embedded Liquidity for ERC-721 Non-Fungible Tokens (NFTs)
description: Standard for embedding liquidity into ERC-721 Non-Fungible Tokens (NFTs) without modifications to ERC-721.
author: AI Protocol <gm@aiprotocol.info>
discussions-to: <URL>
status: Draft
type: Standards Track
category: ERC
created: 2024-02-28
requires: EIP 1671
---

## Abstract

<!--
  The Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.

  TODO: Remove this comment before submitting
-->

This Ethereum Improvement Proposal (EIP) introduces a standard for embedding liquidity into Non-Fungible Tokens (NFTs)
without modifying the ERC721 standard. The proposed standard allows the attachment of an embedded liquidity contract to
an NFT, providing flexibility for implementing various liquidity mechanisms, including bonding curves or other
strategies. The goal is to enable creative and diverse liquidity solutions while ensuring guaranteed fees for creators.

## Motivation

<!--
  This section is optional.

  The motivation section should include a description of any nontrivial problems the EIP solves. It should not describe how the EIP solves those problems, unless it is not immediately obvious. It should not describe why the EIP should be made into a standard, unless it is not immediately obvious.

  With a few exceptions, external links are not allowed. If you feel that a particular resource would demonstrate a compelling case for your EIP, then save it as a printer-friendly PDF, put it in the assets folder, and link to that copy.

  TODO: Remove this comment before submitting
-->

The existing ERC721 standard lacks a specific mechanism for embedding liquidity, limiting the creative possibilities
for NFT-based projects. This EIP addresses the need for a standardized approach to embed liquidity contracts seamlessly
into NFTs, allowing for diverse and innovative implementations without modifying the ERC721 standard.

## Specification

<!--
  The Specification section should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Ethereum platforms (besu, erigon, ethereumjs, go-ethereum, nethermind, or others).

  It is recommended to follow RFC 2119 and RFC 8170. Do not remove the key word definitions if RFC 2119 and RFC 8170 are followed.

  TODO: Remove this comment before submitting
-->

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

1.  Embedding Liquidity Contract:
    - An embedded liquidity contract can be attached to an ERC721 NFT without altering the ERC721 standard.
    - The embedded liquidity contract is responsible for managing liquidity related to the associated NFT.
    - The embedded liquidity contract follows the interface below:

    ```
    /**
     * @title Embedded Liquidity
     *
     * @notice Embedded liquidity stores the liquidity for an NFT; it does so by offering the
     *      mechanism to buy and sell the NFT shares, hence it is also referred to as "tradeable shares"
     *
     * @notice Tradeable shares is a non-transferable, but buyable/sellable fungible token-like asset,
     *      which is sold/bought solely by the shares contract at the predefined by
     *      the bonding curve function price
     *
     * @notice The shares is bound to its "subject" – an NFT; the NFT owner gets the subject fee
     *      emerging in every buy/sell operation
     */
    interface TradeableShares {
        /**
         * @notice Shares subject is an NFT defined by its ERC721 contract address and NFT ID
         */
        struct SharesSubject {
            /// @dev ERC721 contract address
            address tokenAddress;
    
            /// @dev NFT ID
            uint256 tokenId;
        }
    
        /**
         * @dev Fired in `buyShares` and `sellShares` functions, this event logs
         *      the entire trading activity happening on the curve
         *
         * @dev Trader, that is the buyer or seller, depending on the operation type is the transaction sender
         *
         * @param beneficiary the address which receives shares or funds, usually this is the trader itself
         * @param issuer subject issuer, usually an owner of the NFT defined by the subject
         * @param isBuy true if the event comes from the `buyShares` and represents the buy operation,
         *      false if the event comes from the `sellShares` and represents the sell operation
         * @param sharesAmount amount of the shares bought or sold (see `isBuy`)
         * @param paidAmount amount of ETH spent or gained by the buyer or seller;
         *      this is implementation dependent and can represent an amount of ERC20 payment token
         * @param protocolFeeAmount amount of fees paid to the protocol
         * @param holdersFeeAmount amount of fees paid to the shares holders
         * @param subjectFeeAmount amount of fees paid to the subject (issuer)
         * @param supply total shares supply after the operation
         */
        event Trade(
            address indexed beneficiary,
            address indexed issuer,
            bool indexed isBuy,
            uint256 sharesAmount,
            uint256 paidAmount,
            uint256 protocolFeeAmount,
            uint256 holdersFeeAmount,
            uint256 subjectFeeAmount,
            uint256 supply
        );
    
        /**
         * @notice Shares subject, usually defined as NFT (ERC721 contract address + NFT ID)
         *
         * @dev Immutable, client applications may cache this value
         *
         * @return Shares subject as a SharesSubject struct, this is an NFT if all currently known implementations
         */
        function getSharesSubject() external view returns(SharesSubject calldata);
    
        /**
         * @notice Shares issuer, the receiver of the shares fees
         *
         * @dev Mutable, changes (potentially frequently and unpredictably) when the NFT owner changes;
         *      subject to the front-run attacks, off-chain client applications must not rely on this address
         *      in anyway
         *
         * @return nftOwner subject issuer, the owner of the NFT
         */
        function getSharesIssuer() external view returns(address nftOwner);
    
        /**
         * @notice Shares balance of the given holder; this function is similar to ERC20.balanceOf()
         *
         * @param holder the address to check the balance for
         *
         * @return balance number of shares the holder has
         */
        function getSharesBalance(address holder) external view returns(uint256 balance);
    
        /**
         * @notice Total amount of the shares in existence, the sum of all individual shares balances;
         *      this function is similar to ERC20.totalSupply()
         *
         * @return supply total shares supply
         */
        function getSharesSupply() external view returns(uint256 supply);
    
        /**
         * @notice Bonding curve function definition. The function calculating the price
         *      of the `amount` of shares given the current total supply `supply`
         *
         * @param supply total shares supply
         * @param amount number of shares to buy/sell
         * @return the price of the shares (all `amount` amount)
         */
        function getPrice(uint256 supply, uint256 amount) external pure returns(uint256);
    
        /**
         * @notice Total amount of the shares in existence, the sum of all individual shares balances;
         *      this function is similar to ERC20.totalSupply()
         *
         * @return supply total shares supply
         */
        function getSharesSupply() external view returns(uint256 supply);
    
        /**
         * @notice The price of the `amount` of shares to buy calculated based on
         *      the specified total shares supply
         *
         * @param supply total shares supply
         * @param amount number of shares to buy
         * @return the price of the shares to buy
         */
        function getBuyPrice(uint256 supply, uint256 amount) external pure returns(uint256);
    
        /**
         * @notice The price of the `amount` of shares to sell calculated based on
         *      the specified total shares supply
         *
         * @param supply total shares supply
         * @param amount number of shares to sell
         * @return the price of the shares to sell
         */
        function getSellPrice(uint256 supply, uint256 amount) external pure returns(uint256);
    
        /**
         * @notice The price of the `amount` of shares to buy, including all fees;
         *      calculated based on the specified total shares supply and fees percentages
         *
         * @param supply total shares supply
         * @param amount number of shares to buy
         * @param protocolFeePercent protocol fee percent
         * @param holdersFeePercent shares holders fee percent
         * @param subjectFeePercent protocol fee percent
         * @return the price of the shares to buy
         */
        function getBuyPriceAfterFee(
            uint256 supply,
            uint256 amount,
            uint256 protocolFeePercent,
            uint256 holdersFeePercent,
            uint256 subjectFeePercent
        ) external pure returns(uint256);
    
        /**
         * @notice The price of the `amount` of shares to sell, including all fees;
         *      calculated based on the specified total shares supply and fees percentages
         *
         * @param supply total shares supply
         * @param amount number of shares to sell
         * @param protocolFeePercent protocol fee percent
         * @param holdersFeePercent shares holders fee percent
         * @param subjectFeePercent protocol fee percent
         * @return the price of the shares to sell
         */
        function getSellPriceAfterFee(
            uint256 supply,
            uint256 amount,
            uint256 protocolFeePercent,
            uint256 holdersFeePercent,
            uint256 subjectFeePercent
        ) external pure returns(uint256);
    
        /**
         * @notice Current price of the `amount` of shares to buy; calculated based on
         *      the current total shares supply
         *
         * @param amount number of shares to buy
         * @return the price of the shares to buy
         */
        function getBuyPrice(uint256 amount) external view returns(uint256);
    
        /**
         * @notice Current price of the `amount` of shares to sell; calculated based on
         *      the current total shares supply
         *
         * @param amount number of shares to sell
         * @return the price of the shares to sell
         */
        function getSellPrice(uint256 amount) external view returns(uint256);
    
        /**
         * @notice Current price of the `amount` of shares to buy, including all fees;
         *      calculated based on the current total shares supply and fees percentages
         *
         * @param amount number of shares to buy
         * @return the price of the shares to buy
         */
        function getBuyPriceAfterFee(uint256 amount) external view returns(uint256);
    
        /**
         * @notice Current price of the `amount` of shares to sell, including all fees;
         *      calculated based on the current total shares supply and fees percentages
         *
         * @param amount number of shares to sell
         * @return the price of the shares to sell
         */
        function getSellPriceAfterFee(uint256 amount) external view returns(uint256);
    
        /**
         * @notice Buy `amount` of shares. Sender has to supply `getBuyPriceAfterFee(amount)` ETH.
         *      First share can be bought only by current subject issuer.
         *
         * @dev Depending on the implementation, ERC20 token payment may be required instead of ETH.
         *      In such a case, implementation must through if ETH is sent, effectively overriding
         *      the function definition as non-payable
         *
         * @param amount amount of the shares to buy
         */
        function buyShares(uint256 amount) external payable;
    
        /**
         * @notice Buy `amount` of shares in the favor of the address specified (beneficiary).
         *      Sender has to supply `getBuyPriceAfterFee(amount)` ETH.
         *      First share can be bought only by current subject issuer.
         *
         * @dev Depending on the implementation, ERC20 token payment may be required instead of ETH.
         *      In such a case, implementation must through if ETH is sent, effectively overriding
         *      the function definition as non-payable
         *
         * @param amount amount of the shares to buy
         * @param beneficiary an address receiving the shares
         */
        function buySharesTo(uint256 amount, address beneficiary) external payable;
    
        /**
         * @notice Sell `amount` of shares. Sender gets `getSellPriceAfterFee(amount)` of ETH.
         *      Last share cannot be sold.
         *
         * @dev Depending on the implementation, ERC20 token may be payed instead of ETH.
         *
         * @param amount amount of the shares to sell
         */
        function sellShares(uint256 amount) external;
    
        /**
         * @notice Sell `amount` of shares in the favor of the address specified (beneficiary).
         *      The beneficiary gets `getSellPriceAfterFee(amount)` of ETH.
         *      Last share cannot be sold.
         *
         * @dev Depending on the implementation, ERC20 token may be payed instead of ETH.
         *
         * @param amount amount of the shares to sell
         * @param beneficiary an address receiving the funds from the sale
         */
        function sellSharesTo(uint256 amount, address payable beneficiary) external;
    }
    ```

2.  Liquidity Mechanisms:
    - The embedded liquidity contract can implement various liquidity mechanisms, such as bonding curves, liquidity
      locking with ERC20 tokens, or native ETH liquidity pools.
    - The standard allows for flexibility in choosing the liquidity mechanism, depending on the implementation
      requirements.

3.  Guaranteed Creative Fees:
    - The standard emphasizes the inclusion of mechanisms that guarantee creative fees for NFT creators.
    - Creators may specify and receive a percentage of transaction fees generated by the embedded liquidity
      contract during buy and sell operations.

4.  Implementation Independence:
    - The standard is implementation-agnostic, providing developers with the freedom to choose and design the most
      suitable liquidity mechanism for their NFT projects.
    - The embedded liquidity contract can interact with ERC20 tokens or native ETH, depending on the implementation
      requirements.

## Rationale

<!--
  The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

The proposed standard aims to enhance the ERC721 standard by introducing a framework for embedding liquidity contracts
into NFTs. This flexibility encourages creativity and innovation in designing liquidity mechanisms while ensuring that
creators receive guaranteed fees for their contributions.

## Security Considerations

<!--
  All EIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life cycle of the proposal. For example, include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. EIP submissions missing the "Security Considerations" section will be rejected. An EIP cannot proceed to status "Final" without a Security Considerations discussion deemed sufficient by the reviewers.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

1.  Smart Contract Security: Implementations of smart contracts should undergo thorough security audits to ensure
    resistance against vulnerabilities and attacks.

2.  Creative Fee Handling: Mechanisms for handling and distributing creative fees should be secure and transparent to
    prevent any malicious activities.

3.  Compatibility: Developers should ensure compatibility with existing ERC721 implementations, allowing for a smooth
    integration of the embedded liquidity standard.

4.  User Experience: Considerations should be made to maintain a positive user experience, avoiding complexities that
    may hinder the adoption of NFT projects utilizing embedded liquidity.

This proposal is a draft, and further community feedback and collaboration are encouraged to refine and improve the
specifications outlined herein.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
