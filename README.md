# Deploying and Interacting with the DIP721 NFT Container on the Internet Computer

This example demonstrates implementing an NFT canister. 
The canister is a very basic implementation of the standard, we are going to deploy NFT canister and mint Membership NFTs for example usage. 


### Prerequisites

-   [x] Install the [IC SDK](https://internetcomputer.org/docs/current/developer-docs/setup/install/index.mdx).
-   [x] Download and install [git.](https://git-scm.com/downloads)
-   [x] `wasm32-unknown-unknown` targets; these can be installed with `rustup target add wasm32-unknown-unknown`.

### Functions

#### Initialization and Upgrade

-   **`init(args: InitArgs)`**: Initializes the canister with given arguments.
-   **`pre_upgrade()`**: Prepares and serializes the canister's state before an upgrade.
-   **`post_upgrade()`**: Restores the canister's state after an upgrade.

#### NFT Management

-   **`mint(to: Principal, metadata: MetadataDesc, blob_content: Vec<u8>)`**: Mints a new NFT.


#### NFT Transfer

-   **`transfer_from(from: Principal, to: Principal, token_id: u64)`**: Transfers an NFT from one principal to another.
-   **`safe_transfer_from(from: Principal, to: Principal, token_id: u64)`**: Safely transfers an NFT, checking for zero addresses.
-   **`transfer_from_notify(from: Principal, to: Principal, token_id: u64, data: Vec<u8>)`**: Transfers an NFT and notifies the recipient.
-   **`safe_transfer_from_notify(from: Principal, to: Principal, token_id: u64, data: Vec<u8>)`**: Safely transfers an NFT with notification.

#### Query Functions

-   **`balance_of(user: Principal)`**: Returns the number of NFTs owned by a user.
-   **`owner_of(token_id: u64)`**: Returns the owner of a specific NFT.
-   **`name()`**: Returns the name of the NFT collection.
-   **`symbol()`**: Returns the symbol of the NFT collection.
-   **`total_supply()`**: Returns the total number of NFTs minted.
-   **`supported_interfaces()`**: Lists the supported interfaces (DIP721 standards).
-   **`get_metadata(token_id: u64)`**: Retrieves metadata for a specific NFT.
-   **`get_metadata_for_user(user: Principal)`**: Retrieves metadata for all NFTs owned by a user.
-   **`is_principal_member(user: Principal)`**: Returns if user is a member (this is for example implementation).

#### Customization Functions

-   **`set_name(name: String)`**: Sets the name of the NFT collection.
-   **`set_symbol(sym: String)`**: Sets the symbol of the NFT collection.
-   **`set_logo(logo: Option<LogoResult>)`**: Sets the logo for the NFT collection.


## Deployment

To deploy the canister, run the following command. We are going to deploy a sample Membership NFT:

```bash
dfx deploy --argument 'record { name="Membership"; symbol="MMBR"; custodians=null; logo=null;  }' dip721_nft_container
```

## Minting Tokens

To mint a new token, run the following command:

```bash
dfx canister call dip721_nft_container mintDip721 \
"(principal\"$YOU\",vec{record{
    purpose=variant{Rendered};
    data=blob\"hello\";
    key_val_data=vec{
        record{
            \"MembershipType\";
            variant{TextContent=\"Gold"}; 
        };
    }
}},blob\"hello\")"
```

Replace `$YOU` with your principal. This will mint a new token with a "Gold" membership type.

You should see the following output:

```bash
(variant { Ok = record { id = 0 : nat; token_id = 0 : nat64 } })
```

To get your principal id, run the folllowing command:
```bash
dfx identity get-principal
```

## Retrieving Metadata

To retrieve the metadata for a token, run the following command:

```bash
dfx canister call dip721_nft_container getMetadata "(0: nat64)"
```

Example output:

```bash
(
  variant {
    Ok = vec {
      record {
        data = blob "hello";
        key_val_data = vec {
          record { "MembershipType"; variant { TextContent = "Gold" } };
        };
        purpose = variant { Rendered };
      };
    }
  },
)
```

## Checking Membership

To check if a principal is a member, run the following command:

```bash
dfx canister call dip721_nft_container isPrincipalMember "(principal\"c34g5-fdzyx-hoqia-mrhsl-krips-3asow-bjuqe-rtpy7-ene2q-atrbo-kae\")"
```

Replace the principal with the one you want to check. This will return `true` if the principal is a member, and `false` otherwise.

## Candid UI

You can also use the [Candid UI](https://sdk.dfinity.org/docs/candid-guide/candid-ui.html) to interact with the canister.

![image](https://github.com/mervanerenci/ICP_NFT_DIP721/assets/101268022/1c610548-c82d-4796-b102-ca76db872d77)


