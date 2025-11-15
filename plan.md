# BIP-39 Seed Phrase Recovery Tool - Implementation Plan

## Project Overview
A browser-based application that helps users recover lost BIP-39 seed phrase words by brute-forcing the missing words and finding valid combinations with correct checksums.

## Core Requirements
- **Input**: Partial BIP-39 seed phrase (less than 24 words)
- **Processing**: Complete the phrase to 24 words using BIP-39 word list
- **Validation**: Verify checksum correctness for each combination
- **Output**: Display valid phrases with their first derived Ethereum address (Ledger derivation path)

## Technical Specifications

### BIP-39 Details
- Total word list: 2048 words
- Seed phrase: 24 words (256 bits of entropy + 8 bit checksum)
- Each word represents 11 bits
- Last word contains checksum bits

### Ethereum Derivation Path
- Use Ledger Live derivation path: `m/44'/60'/0'/0/0`
- Generate first address from this path

## Implementation Steps

### 1. Project Setup
- Create a single HTML file application (for simplicity)
- Include necessary JavaScript libraries via CDN:
  - `ethers.js` or `ethereum-cryptography` for BIP-39 and HD wallet operations
  - Optionally: A lightweight CSS framework for basic styling

### 2. User Interface
Create a simple, clean interface with:
- **Header**: "BIP-39 Seed Recovery Tool"
- **Input field**: 
  - Textarea for entering known words (space or comma separated)
  - Placeholder: "Enter your known seed words..."
- **Submit button**: 
  - Text: "Recover Missing Words"
  - Disabled state while processing
- **Results section**:
  - Initially hidden
  - Table or cards showing recovered combinations
  - Each result shows: complete phrase and derived address
- **Progress indicator**: Show processing status

### 3. Core Functionality Implementation

#### 3.1 Input Processing
```javascript
function parseInput(inputText) {
  // Clean and split input into words
  // Validate each word against BIP-39 wordlist
  // Return array of valid words or throw error
}
```

#### 3.2 BIP-39 Word List
```javascript
// Load the complete BIP-39 English wordlist (2048 words)
// Can be hardcoded or loaded from external source
const BIP39_WORDLIST = [...];
```

#### 3.3 Phrase Completion Algorithm
```javascript
function generateCompletions(knownWords) {
  // Calculate number of missing words (24 - knownWords.length)
  // Generate all possible combinations from BIP-39 wordlist
  // For optimization: implement iterative generation to avoid memory issues
}
```

#### 3.4 Checksum Validation
```javascript
function isValidMnemonic(phrase) {
  // Convert mnemonic to entropy
  // Validate checksum bits
  // Return true if valid, false otherwise
}
```

#### 3.5 Address Derivation
```javascript
function deriveAddress(mnemonic) {
  // Create seed from mnemonic
  // Derive HD wallet using path: m/44'/60'/0'/0/0
  // Return first Ethereum address
}
```

#### 3.6 Main Recovery Function
```javascript
async function recoverSeedPhrase(knownWords) {
  const results = [];
  const missingCount = 24 - knownWords.length;
  
  // Iterate through possible combinations
  // For each combination:
  //   1. Create complete phrase
  //   2. Check if mnemonic is valid
  //   3. If valid, derive address
  //   4. Add to results
  
  // Performance optimization:
  // - Use Web Workers for parallel processing if many combinations
  // - Implement batching to prevent UI freezing
  // - Add progress updates
  
  return results;
}
```

### 4. Performance Considerations
- **Brute force limits**: 
  - 1 missing word: 2048 combinations (instant)
  - 2 missing words: 4,194,304 combinations (few seconds)
  - 3 missing words: 8,589,934,592 combinations (may take minutes)
  - Display warning for >3 missing words
  
- **Optimizations**:
  - Use Web Workers for parallel processing
  - Implement progressive rendering of results
  - Add "Stop" button for long operations
  - Cache derived addresses to avoid recalculation

### 5. Error Handling
- Validate input words against BIP-39 wordlist
- Show clear error messages for invalid inputs
- Handle edge cases (empty input, too few words, etc.)
- Implement timeout for long operations

### 6. Security Considerations
- Add prominent warning: "Only use this tool offline or on an air-gapped computer"
- No external API calls or data transmission
- Clear warnings about seed phrase security
- Option to clear all data from page

## File Structure
```
word-recoverer/
├── index.html          # Single-page application
├── README.md           # Usage instructions and warnings
└── plan.md            # This file
```

## HTML Structure Template
```html
<!DOCTYPE html>
<html>
<head>
    <title>BIP-39 Seed Recovery Tool</title>
    <style>
        /* Basic styling */
    </style>
</head>
<body>
    <div class="container">
        <h1>BIP-39 Seed Recovery Tool</h1>
        
        <div class="warning">
            ⚠️ Security Warning: Use offline only!
        </div>
        
        <div class="input-section">
            <textarea id="seedInput" placeholder="Enter known words..."></textarea>
            <button id="recoverBtn">Recover Missing Words</button>
        </div>
        
        <div id="progress" class="hidden">
            <div class="progress-bar"></div>
            <span class="progress-text"></span>
        </div>
        
        <div id="results" class="hidden">
            <h2>Valid Combinations Found:</h2>
            <div id="resultsList"></div>
        </div>
    </div>
    
    <script>
        // All JavaScript implementation here
    </script>
</body>
</html>
```

## Testing Checklist
- [ ] Test with valid partial seed phrases
- [ ] Test with 1, 2, and 3 missing words
- [ ] Verify checksum validation works correctly
- [ ] Confirm correct address derivation
- [ ] Test error handling for invalid inputs
- [ ] Verify UI responsiveness during processing
- [ ] Test stop/cancel functionality
- [ ] Validate security warnings are prominent

## Libraries and Resources
- **ethers.js**: `https://cdn.jsdelivr.net/npm/ethers@6/dist/ethers.umd.min.js`
- **BIP-39 Word List**: Can be found in ethers.js or as standalone JSON
- **Reference**: 
  - BIP-39 Specification: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
  - BIP-44 (HD Wallets): https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki

## Implementation Notes
1. Start with a simple version that works for 1-2 missing words
2. Add optimizations for handling more missing words
3. Ensure all cryptographic operations use well-tested libraries
4. Focus on user safety and security warnings
5. Make the UI intuitive and provide clear feedback during processing

## Success Criteria
- Application correctly recovers seed phrases with 1-3 missing words
- Valid mnemonics pass checksum validation
- Derived addresses match expected Ledger derivation path
- UI remains responsive during processing
- Clear security warnings are displayed
- Results are presented in a user-friendly format
