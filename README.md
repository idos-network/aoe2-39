# 🔐 BIP-39 Seed Recovery Tool

A browser-based application that helps recover lost BIP-39 seed phrase words by brute-forcing the missing words and finding valid combinations with correct checksums.

## ⚠️ CRITICAL SECURITY WARNING

**ONLY USE THIS TOOL ON AN OFFLINE, AIR-GAPPED COMPUTER!**

- Never enter real seed phrases on an internet-connected device
- Your seed phrase controls your cryptocurrency assets
- This tool should only be used in a secure, offline environment
- Entering sensitive seed phrases on an online computer can lead to complete loss of funds

## What This Tool Does

This tool helps you recover a complete 24-word BIP-39 seed phrase when you have most of the words but are missing a few. It:

1. Takes your known seed words as input (less than 24 words)
2. Generates all possible combinations to complete the phrase to 24 words
3. Validates each combination using BIP-39 checksum verification
4. Derives the first Ethereum address using the Ledger derivation path (`m/44'/60'/0'/0/0`)
5. Displays all valid combinations with their corresponding addresses

## How to Use

### Basic Usage

1. **Download this repository** and open `index.html` in a modern web browser
2. **Enter your known seed words** in the text field (space or comma separated)
3. Click **"Recover Missing Words"**
4. Wait for the tool to find valid combinations
5. Review the results showing complete phrases and derived addresses

### Performance Expectations

The number of missing words affects both total combinations and valid results:

#### Understanding BIP-39 Checksums

BIP-39 24-word phrases consist of:
- **256 bits of entropy** (random data)
- **8 bits of checksum** (derived from entropy via SHA-256)
- Total: 264 bits = 24 words × 11 bits each

**The checksum dramatically reduces the search space:**

- **1 missing word** (11 bits):
  - Total combinations: 2,048
  - Valid phrases: ~8 (1 in 256, due to 8-bit checksum)
  - Time: < 1 second

- **2 missing words** (22 bits):
  - Total combinations: 4,194,304
  - Valid phrases: ~16,384 (2^14, because 8 bits are checksum)
  - Time: Few seconds

- **3 missing words** (33 bits):
  - Total combinations: 8,589,934,592
  - Valid phrases: ~33,554,432 (2^25)
  - Time: Several minutes

- **4+ missing words**: May take hours/days or crash your browser

**Why the reduction?** The 8-bit checksum must match the entropy. This eliminates 255 out of every 256 random combinations, significantly reducing valid results compared to total combinations tested.

### Tips for Best Results

- Make sure all entered words are spelled correctly
- Words should be from the standard BIP-39 English word list
- The tool validates words as you enter them
- You can stop the recovery process at any time using the "Stop" button
- Results are displayed as they are found

## Example Test Case

To test the tool without using real seed phrases, try this example:

**Enter these 23 words:**
```
abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon
```

The tool should find the valid completion with the 24th word.

## Technical Details

### Technologies Used

- **ethers.js v6**: For BIP-39 mnemonic validation and HD wallet derivation
- **Pure JavaScript**: No build process required
- **Client-side only**: All processing happens in your browser

### How It Works

1. **Input Validation**: Checks each word against the BIP-39 word list (2048 words)
2. **Combination Generation**: Creates all possible completions using remaining words
3. **Checksum Validation**: Uses BIP-39 checksum to verify valid mnemonics
4. **Address Derivation**: Derives Ethereum address using Ledger Live path
5. **Real-time Results**: Shows valid combinations as they are found

### BIP-39 Specification

- Seed phrases use a specific list of 2048 words
- Each word represents 11 bits of entropy
- Last word contains checksum bits
- 24 words = 256 bits of entropy + 8-bit checksum

### Ethereum Derivation

- Uses HD wallet derivation per BIP-44
- Ledger Live path: `m/44'/60'/0'/0/0`
- Generates the first address from the derived wallet

## Security Features

- ✅ Runs entirely in your browser
- ✅ No network requests or external API calls
- ✅ No data transmission
- ✅ Prominent security warnings
- ✅ Can be used offline

## Limitations

- Only supports 24-word seed phrases
- Only supports English BIP-39 word list
- Only derives Ethereum addresses (EVM compatible)
- Performance degrades exponentially with more missing words
- Browser may crash with 4+ missing words due to memory constraints

## Troubleshooting

### "Invalid BIP39 words found" Error
- Check spelling of each word
- Ensure words are from the BIP-39 word list
- Remove any extra spaces or punctuation

### Browser Becomes Unresponsive
- You may have too many missing words
- Try stopping the process and reducing missing words
- Close other browser tabs to free up memory

### No Valid Combinations Found
- Verify that the entered words are correct
- Check that words are in the correct order
- The seed phrase may have errors in the known words

## Development

This is a single-file application for simplicity and security. The complete implementation is in `index.html`.

### File Structure
```
word-recoverer/
├── index.html    # Complete application
├── README.md     # This file
└── plan.md       # Implementation plan and specifications
```

### Running Locally

Simply open `index.html` in any modern web browser. No server or build process needed.

For security testing, you can:
1. Disconnect from the internet
2. Open the file locally
3. Test with known seed phrases

## FAQ

**Q: Is this tool safe to use?**  
A: Only if used offline on an air-gapped computer. Never use it on an internet-connected device with real seed phrases.

**Q: How long will recovery take?**  
A: Depends on missing words: 1 word = instant, 2 words = seconds, 3 words = minutes to hours.

**Q: Can I recover passwords or passphrases?**  
A: No, this only recovers BIP-39 seed phrase words, not passwords.

**Q: Will this work for Bitcoin or other cryptocurrencies?**  
A: The checksum validation works for any BIP-39 phrase, but address derivation is currently Ethereum-only.

**Q: What if I have more than 3 missing words?**  
A: The tool will warn you and may take an extremely long time or crash. Consider if you have other information to narrow down possibilities.

## License

This tool is provided as-is for educational and recovery purposes. Use at your own risk.

## Disclaimer

- This software is provided without warranty
- Always verify recovered addresses match your expected wallet
- Test with small amounts before using recovered seeds
- The authors are not responsible for any loss of funds
- This tool does not guarantee successful recovery

---

**Remember: Your seed phrase is the master key to your cryptocurrency. Treat it with extreme care and only use this tool in a secure, offline environment.**
