# Design Analysis

## Version 2 Diagram

![Commodities Market Ticker v2 Diagram](https://mermaid.ink/img/pako:eNrNVctu2zAQ_BWCQAAbtXMo0B6E1IDzKOBDiqDxUZctuVKIiKRKUUiExP9eSiIlynbcnILwRC1nh7MPrl4o0xxpQlkBVXUtIDcgU0Xc6izkSkupubACq1swj2i3gj2iIS89qF1fgHODDqufFJrIXgtlv377ThQ-28DTbPgbiDVjulZ2ci6hLIXKZwH3YzUIauaEjdpO-2w895xAv4nxzCBY9IhZZY1zDrhtU-KCeJtQmZ4TT3tAMOgKFAqk8w0qSiNY9Pm3BmUdeCBnjiLXpjl2AT4jqy1uDXAcwvpTN2g2fKSssCimFjbm_PDmeVyGkrvr71qJPxF5iGDw_9WFEsqcOci630ckS5TCdhJverl8dlLHO_SPSZpk0V-6S1XcqKHGcWdeXDg_NBkwXK2ieHN0bTY7lur2xJXZnfU5mB6tx67YQ-yJuWyDm7yREIGI2ztqrI9Ud99l-tPKG17SuxW2T-0ItOuWI_bQV4dE4RXu6To7I7-xACu0qh5E6afHW6NxuRwnDkmIBAV5mFB9Y5yfv04gQpYFShzmki_Q_2CnBIxJTEghqtaFLqhEI0FwN--71KbUPjjClCZuyzGDurApTdXOQaG2-r5RjCYZFBUuaD8k_C_CW3f_AAsQ-kw)

## Trade Part

[![](https://mermaid.ink/img/pako:eNq9lE1vwjAMhv9KlDNcJm2H3hgwaRoMxscBqReTuBApHyVNYBXw35e2FNjUiWmTlkuc6Hlt13a6p8xwpBFF2xOwsqBiTcJ6nC_6E3I4tNtmT2aTTq9PIoLvyLzDrEKm_cHgFtMdDYej3vNs8RUTemvktsaqaPvqUCwvtLu7fyCCk_ELielci43HcETtRCLQxvQCZ84KvQouExPQDufCCaNBljdWQXGq-eOn3P8x4qUSfwyqQWFAX4vNJMStkTCjlAk55Nd47Tu1ghWCcbmnaInXwjWRGw8hrMsD_FabsAUhYSmxIRMGDlfGFny3Nr_L6FSFqv2_rUANL32O9pmTp0IxwQQtaoZZNUZNggylbFRUc9AkOX9Bg-rcy59W0VngyG93BxzZrQVbVwKyg6x-T41iJxRmDlQaHMzO9qkFpYur8tMWVRhmU_Dw2ssOxDRwKjBRMDkm4GU5F8eAgndmmmtGowRkhi3qUx56fPpHnG6PHzcjUwo?type=png)](https://mermaid.live/edit#pako:eNq9lE1vwjAMhv9KlDNcJm2H3hgwaRoMxscBqReTuBApHyVNYBXw35e2FNjUiWmTlkuc6Hlt13a6p8xwpBFF2xOwsqBiTcJ6nC_6E3I4tNtmT2aTTq9PIoLvyLzDrEKm_cHgFtMdDYej3vNs8RUTemvktsaqaPvqUCwvtLu7fyCCk_ELielci43HcETtRCLQxvQCZ84KvQouExPQDufCCaNBljdWQXGq-eOn3P8x4qUSfwyqQWFAX4vNJMStkTCjlAk55Nd47Tu1ghWCcbmnaInXwjWRGw8hrMsD_FabsAUhYSmxIRMGDlfGFny3Nr_L6FSFqv2_rUANL32O9pmTp0IxwQQtaoZZNUZNggylbFRUc9AkOX9Bg-rcy59W0VngyG93BxzZrQVbVwKyg6x-T41iJxRmDlQaHMzO9qkFpYur8tMWVRhmU_Dw2ssOxDRwKjBRMDkm4GU5F8eAgndmmmtGowRkhi3qUx56fPpHnG6PHzcjUwo)

## Discussion

The `CommoditiesMarketTicker` contract is architected for efficiency and scalability. It minimizes on-chain storage costs by leveraging events to log trade data, utilizing The Graph for off-chain indexing and querying. This strategy accommodates a high volume of transactions without incurring significant costs associated with on-chain data storage.

In the current early version, Chainlink integration is omitted to simplify the implementation. Future versions will progressively integrate Chainlink's VRF and price feeds, which will introduce more complexity but provide additional decentralized capabilities.

## Example of a Simple User Journey (Offchain + Onchain)

1. **Offchain: Seller's Perspective**
   - Seller logs into the platform's web interface.
   - Seller navigates to "List New Commodity" section and fills out the form with maize details.
   - Seller submits the form, triggering an offchain request to the backend.

2. **Onchain: Listing the Commodity**
   - Backend sends a transaction to `CommoditiesMarketTicker` calling `createCommodity`.
   - Contract emits `CommodityCreated` event upon successful listing.

3. **Offchain: Buyer's Perspective**
   - Buyer logs into the platform and browses commodities.
   - Buyer selects maize listing, enters quantity, and confirms purchase.
   - This action triggers an offchain request to the backend.

4. **Onchain: Executing the Trade**
   - Backend sends a transaction to `CommoditiesMarketTicker` calling `executeTrade`.
   - Contract checks funds and availability, then emits `TradeExecuted` event.

5. **Offchain: Post-Trade**
   - The Graph indexes `TradeExecuted` event.
   - Seller and buyer are notified via the platform's web interface.
   - Platform updates the listed maize quantity or removes the listing if sold out.

[![](https://mermaid.ink/img/pako:eNqdlM1u4jAUhV_F8orRhBfIAqmkqKo0rMoyG499IRaJnfqnagbx7nNtJwVCKDOwSq7POffmw_aBci2A5tTCuwfF4VmynWFNqQj-Wmac5LJlypEn0Uh1XS7WG8IsKXTTaCGdBLtmZg9uI_kezLV-WQT5stZ8zys2lfhiWFsF0aaC9HKteYO6nkz3XSinhTjxfLH4iTPmhBtgDoY5u1lJ10z-gZJmpDWSQ0bePUbgUkZKin1xNvojJWFAyFkWOVk10n19bVfEUJFUy2KOqnnstvoAHKfWu92wGjLCcpwqP0X0gwliPedg7dbXdZcs8ePRE_r-ktaBIk7fbr5YRANmM8erKx2BMNN58snxqgR8nhwDwYR5hDAVZ1Jt9Q08SfEAm2TM-wDCONcepfw8aRrKdMsRkQvRfRzDJuvjwtYaoYi170hEwQMgoi9P9v_BMNlvROFccx_CxYm6ZACfwL2DjWECZr_D0qvIiI3UwhMftlN4GQ7XDVAxZJUCHyEV_cNE_36WptuOgF2I7hOL8lLRjDZgGiYF3q6HYCipq6DBGyfHRwFb5mtX0lIdUcq802-d4jTfstpCRn0r8A_qb-O-evwLm4zdAw?type=png)](https://mermaid.live/edit#pako:eNqdlM1u4jAUhV_F8orRhBfIAqmkqKo0rMoyG499IRaJnfqnagbx7nNtJwVCKDOwSq7POffmw_aBci2A5tTCuwfF4VmynWFNqQj-Wmac5LJlypEn0Uh1XS7WG8IsKXTTaCGdBLtmZg9uI_kezLV-WQT5stZ8zys2lfhiWFsF0aaC9HKteYO6nkz3XSinhTjxfLH4iTPmhBtgDoY5u1lJ10z-gZJmpDWSQ0bePUbgUkZKin1xNvojJWFAyFkWOVk10n19bVfEUJFUy2KOqnnstvoAHKfWu92wGjLCcpwqP0X0gwliPedg7dbXdZcs8ePRE_r-ktaBIk7fbr5YRANmM8erKx2BMNN58snxqgR8nhwDwYR5hDAVZ1Jt9Q08SfEAm2TM-wDCONcepfw8aRrKdMsRkQvRfRzDJuvjwtYaoYi170hEwQMgoi9P9v_BMNlvROFccx_CxYm6ZACfwL2DjWECZr_D0qvIiI3UwhMftlN4GQ7XDVAxZJUCHyEV_cNE_36WptuOgF2I7hOL8lLRjDZgGiYF3q6HYCipq6DBGyfHRwFb5mtX0lIdUcq802-d4jTfstpCRn0r8A_qb-O-evwLm4zdAw)


## Code Repository

The code repository for the project is available at:
[Commodities Market Ticker V2](https://github.com/market-ticker/CMT/tree/v2)

## Contracts

### Commodity

- **Contract Name**: `Commodity`
- **Address**: [0x5062434556bcf4ce41f71c38BD9D4f57111d690E](https://sepolia.etherscan.io/address/0x5062434556bcf4ce41f71c38BD9D4f57111d690E#code)
- **Description**: This contract represents a commodity, with properties such as ID, name, price, quantity, and category.

### Buyer

- **Contract Name**: `Buyer`
- **Address**: [0x65d9cDe879b67d20D121838800EC087891a26365](https://sepolia.etherscan.io/address/0x65d9cDe879b67d20D121838800EC087891a26365#code)
- **Description**: This contract represents a buyer, with properties and methods to manage buyer information.

### Seller

- **Contract Name**: `Seller`
- **Address**: [0xF6E2B08f2dDC9e0805Eaf02768E41310d5bcaFE7](https://sepolia.etherscan.io/address/0xF6E2B08f2dDC9e0805Eaf02768E41310d5bcaFE7#code)
- **Description**: This contract represents a seller, with properties and methods to manage seller information.

### CommoditiesMarketTicker

- **Contract Name**: `CommoditiesMarketTicker`
- **Address**: [0xF242Ab57dDB64e2AA8c2143AD08913823f26EDfa](https://sepolia.etherscan.io/address/0xF242Ab57dDB64e2AA8c2143AD08913823f26EDfa#code)
- **Description**: This is the main contract for the commodities market ticker, handling the creation of buyer and seller accounts, commodities, and the execution of trades.

---

Please note that all contracts are deployed on the Sepolia test network and verified on Sepolia Etherscan.

## Future Evolution

- Deploy on the Sepolia testnet to validate functionality.(done)
- Migrate to Polygon zkEVM for enhanced scalability and lower transaction costs.
- Implement account abstraction to simplify user experience and enhance security.
- Integrate Chainlink to provide verifiable randomness and real-time price feeds for commodities.

Each step will be carefully tested to ensure the integrity and performance of the platform are maintained as new features are integrated.

