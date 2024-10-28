# Supply Chain Tracking Smart Contract

A Clarity smart contract implementation for tracking products through their lifecycle in a supply chain using the Stacks blockchain. This contract enables transparent product tracking, ownership transfers, and status updates while maintaining an immutable history of all changes.

## Features

- **Product Registration**: Securely register new products with detailed information
- **Ownership Management**: Transfer product ownership between supply chain participants
- **Status Tracking**: Update and track product status throughout the supply chain
- **Location Tracking**: Record product locations at each stage
- **Complete History**: Maintain an immutable record of all product changes and transfers
- **Access Control**: Secure functions with ownership and authorization checks

## Contract Functions

### Administrative Functions

```clarity
(register-product (product-id (string-ascii 36)) (name (string-ascii 64)) (location (string-ascii 100)))
```
- Registers a new product in the system
- Only callable by contract owner
- Returns: `(ok true)` on success

### Public Functions

```clarity
(transfer-ownership (product-id (string-ascii 36)) (new-owner principal) (location (string-ascii 100)) (notes (string-ascii 200)))
```
- Transfers product ownership to a new party
- Only callable by current owner
- Returns: `(ok true)` on success

```clarity
(update-status (product-id (string-ascii 36)) (new-status (string-ascii 20)) (location (string-ascii 100)) (notes (string-ascii 200)))
```
- Updates product status and location
- Only callable by current owner
- Returns: `(ok true)` on success

### Read-Only Functions

```clarity
(get-product-details (product-id (string-ascii 36)))
```
- Returns current product information
- Public access

```clarity
(get-history-entry (product-id (string-ascii 36)) (index uint))
```
- Returns specific historical entry
- Public access

```clarity
(get-history-length (product-id (string-ascii 36)))
```
- Returns number of historical entries for a product
- Public access

## Data Structures

### Product Information
```clarity
{
    manufacturer: principal,
    current-owner: principal,
    name: (string-ascii 64),
    status: (string-ascii 20),
    timestamp: uint,
    location: (string-ascii 100)
}
```

### History Entry
```clarity
{
    owner: principal,
    status: (string-ascii 20),
    timestamp: uint,
    location: (string-ascii 100),
    notes: (string-ascii 200)
}
```

## Error Codes

- `u100`: Operation restricted to contract owner
- `u101`: Product not found
- `u102`: Unauthorized operation
- `u103`: Invalid status update

## Setup and Deployment

1. Install Clarinet and other Stacks development tools
2. Clone the repository
3. Test the contract using Clarinet:
   ```bash
   clarinet test
   ```
4. Deploy to testnet/mainnet using Clarinet:
   ```bash
   clarinet deploy --network testnet
   ```

## Usage Example

1. Register a new product:
```clarity
(contract-call? 
    .supply-chain-tracking 
    register-product 
    "prod123" 
    "Organic Coffee Beans" 
    "Coffee Farm, Colombia"
)
```

2. Update product status:
```clarity
(contract-call? 
    .supply-chain-tracking 
    update-status 
    "prod123" 
    "in-transit" 
    "Port of Miami" 
    "Shipped via container ABC123"
)
```

3. Transfer ownership:
```clarity
(contract-call? 
    .supply-chain-tracking 
    transfer-ownership 
    "prod123" 
    'SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7 
    "Warehouse, Miami" 
    "Transferred to distributor"
)
```

## Security Considerations

1. Only the contract owner can register new products
2. Only the current owner can transfer ownership or update status
3. All historical data is immutable once recorded
4. Timestamps are based on block height for consistency

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contact

For questions and support, please open an issue in the GitHub repository.

---