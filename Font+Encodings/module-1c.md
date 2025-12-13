# Module 1C: Your Diagnostic Toolkit

**Duration:** 45-60 minutes  
**Level:** Has Knowledge  
**Goal:** Develop practical diagnostic skills and prepare for Course 2's hands-on troubleshooting

---

## Connect (8-10 minutes)

### From Understanding to Action

In Module 1A, you learned to **recognize** broken text patterns.  
In Module 1B, you learned to **understand** why they happen.

Now it's time to build your **diagnostic toolkit**—the practical skills and resources you need to investigate problems systematically.

### The Confident Diagnosis

Imagine this scenario:

A colleague messages you: "Help! I have a wordlist file that looks completely broken. Can you fix it?"

With what you've learned so far, you might think: "Is it encoding? Font? Both?"

But **how do you actually check?** How do you look "under the hood" to see what's really going on?

That's what this module is about—moving from educated guesses to **confident diagnosis**.

### Your Current Toolkit

Take a moment to think:

**What tools do you currently use when you encounter a file problem?**
- Do you just try different fonts randomly?
- Do you open files in different programs hoping one works?
- Do you have a systematic approach?

*(2 minutes for reflection or discussion)*

Most people start by trial and error. That's fine for learning—but as a language technology consultant, you need **diagnostic methods** you can teach to others and apply consistently.

---

## Content (30-35 minutes)

### Tool 1: Inspecting File Encoding

#### Why You Need This

When you see mojibake, you need to know: "What encoding is this file actually in?"

You can't fix an encoding problem if you're just guessing.

#### How to Check Encoding

**Method 1: Notepad++ (Windows)**

Notepad++ is a free text editor that shows you the file's encoding:

1. Open the file in Notepad++
2. Look at the bottom right of the window
3. You'll see: "UTF-8", "ANSI", "UTF-8-BOM", etc.

**What you're seeing:** The encoding Notepad++ *detected* (not always what it *should* be)

If the text looks broken, try:
- Menu: Encoding → Convert to UTF-8
- Or: Encoding → Character sets → (try different ones to see if text becomes readable)

**Method 2: File Command (Mac/Linux)**

In Terminal:
```
file -I filename.txt
```

This shows the detected encoding. Again, this is an educated guess by the system—not always correct if the file is misencoded.

**Method 3: Looking at the Raw Bytes**

For advanced diagnosis, you can open a file in a hex editor to see the actual numbers stored. This is beyond "Has Knowledge" level, but it's good to know it exists.

#### Activity: Encoding Detective Work (8 minutes)

**If you have access to sample files:**

1. Download these two sample files: [sample-utf8.txt] and [sample-windows1252.txt]  
   *(Implementation note: Course developers should provide these)*

2. Open each in Notepad++ (or your text editor)

3. Check the encoding shown

4. Try deliberately changing the encoding (Encoding menu → Character sets) and watch what happens to the text

**What you should notice:**
- UTF-8 files with non-ASCII characters will break if read as Windows-1252
- Windows-1252 files will show mojibake if read as UTF-8
- The *same file* looks completely different depending on which encoding you use to read it

**If you don't have sample files right now:**

Think through this scenario: You receive a file that shows `cafÃ©` instead of `café`. 

- What encoding was it probably saved in? (UTF-8)
- What encoding is it being read as? (Windows-1252)
- How would you fix it? (Open in editor that lets you choose encoding, select UTF-8)

**Key insight:** Encoding problems aren't about the *file being wrong*—they're about reading it with the wrong decoder.

---

### Tool 2: Checking Font Coverage

#### Why You Need This

When you see boxes, you need to know: "Does this font actually have these characters?"

You can't assume—you need to check.

#### Method 1: SIL Font Documentation

Every SIL font has a documentation page that lists character coverage:

**Example: Charis SIL**
1. Go to https://software.sil.org/charis/
2. Click "Character Set Support" or "About"
3. You'll see which Unicode blocks are included

**What you're looking for:**
- "IPA Extensions" (for phonetic symbols)
- "Latin Extended-A, -B, -C, -D" (for diacritics and special Latin)
- Specific scripts (Cyrillic, Greek, etc.)

#### Method 2: Type and See

The simplest test:
1. Open a text editor or word processor
2. Type (or paste) the problem characters
3. Change fonts and see which ones display them correctly

**Systematic testing:**
- Try Arial (baseline—limited coverage)
- Try Times New Roman (slightly better)
- Try Charis SIL or Doulos SIL (comprehensive linguistic coverage)
- Try Gentium Plus (excellent for extended Latin)

#### Method 3: Character Map/Font Book

**Windows:** Character Map utility  
**Mac:** Font Book application  
**Linux:** GNOME Character Map

These tools let you browse through a font and see every character it contains.

**How to use:**
1. Open Character Map
2. Select the font you're investigating
3. Scroll through to see if your needed characters are present
4. Look for the specific Unicode value if you know it

#### Activity: Font Coverage Comparison (10 minutes)

**Step 1:** Go to https://software.sil.org/fonts/ and explore three fonts:
- Charis SIL
- Doulos SIL  
- Andika

**Step 2:** For each one, look at the "Character Set Support" section

**Step 3:** Answer these questions:
- Do all three include full IPA coverage?
- Which one is designed specifically for beginning readers? (Hint: Look at Andika's description)
- Which one would you recommend for: (a) academic publications, (b) literacy materials, (c) screen display?

**Step 4:** If you have these fonts installed, type this string and see how each font displays it:
```
[ŋ ʈ ɲ ɔ̃ː ṵ̂]
```

**Answers to Step 3:**
- Yes, all three include full IPA
- Andika (designed for clarity in literacy contexts)
- Recommendations: (a) Charis or Doulos for academic, (b) Andika for literacy, (c) any work well on screen, but Charis is optimized for it

**Key insight:** Different fonts serve different purposes. It's not just "can it display the characters?"—it's also "is it appropriate for this use case?"

---

### Tool 3: Understanding Unicode Values

#### Why You Need This

Sometimes you need to identify *exactly* which character you're looking at—especially when:
- Two characters look identical but behave differently
- You need to verify precomposed vs. decomposed forms
- You're reporting a problem and need to be precise

#### What Unicode Values Look Like

Every character has a unique code point written as: **U+XXXX**

Examples:
- `a` = U+0061
- `é` (precomposed) = U+00E9
- `e` + combining acute = U+0065 + U+0301
- `ŋ` (eng) = U+014B

#### How to Find Unicode Values

**Method 1: Unicode websites**
- Go to https://unicode.org/charts/
- Find your character and see its code point
- Or search: "unicode latin small letter eng" → U+014B

**Method 2: Unibook (SIL Tool)**
- Free tool from SIL that shows Unicode info for any text
- Paste text and it shows you the exact code points
- Download at: https://scripts.sil.org/unibook

**Method 3: Your text editor**
- Some editors (like Notepad++) show Unicode values when you hover over characters
- Or position cursor and check status bar

#### Activity: Character Identity Check (5 minutes)

**Scenario:** A colleague shows you two files. In both files, you see what *looks* like the letter "a" with a tilde: "ã"

But in File 1, it's one character (precomposed).  
In File 2, it's two characters (base + combining).

**How would you verify this?**

1. Copy-paste the character into a text editor
2. Use arrow keys to move the cursor—does it take one keystroke or two to move past it?
3. Use Unibook or a Unicode identifier tool to see the code points

**File 1:** U+00E3 (one code point = precomposed)  
**File 2:** U+0061 + U+0303 (two code points = decomposed)

Both should display the same, but they behave differently when:
- Searching
- Sorting
- Converting between programs
- Counting characters

**Key insight:** Visual appearance doesn't tell you everything. Sometimes you need to see the actual Unicode values to understand what's really in the file.

---

### Tool 4: Documentation and Help Resources

#### Building Your Reference Library

As a language technology consultant, you won't memorize everything—but you need to know **where to find answers**.

**Essential bookmarks:**

1. **SIL Fonts**  
   https://software.sil.org/fonts/  
   Your primary source for fonts with comprehensive linguistic character support

2. **SIL Scripts and Unicode**  
   https://scripts.sil.org/  
   Technical documentation on Unicode, encoding, and script support

3. **Unicode Standard**  
   https://unicode.org/standard/standard.html  
   The official specification (very technical, but authoritative)

4. **Unicode Charts**  
   https://unicode.org/charts/  
   Visual reference for all Unicode characters organized by script/block

5. **SIL Language Technology Tools**  
   https://software.sil.org/  
   Home page for all SIL's linguistic software tools

#### When to Search vs. When to Know

**You should know:**
- The three main culprits (encoding, font, rendering)
- Common patterns (boxes, wrong marks, mojibake)
- Where to find SIL fonts
- Basic troubleshooting sequence

**You should know where to look up:**
- Specific Unicode values
- Whether a particular font supports a specific character
- How to convert between specific encodings
- Technical details about normalization forms

**You don't need to memorize:**
- Every Unicode block
- Every encoding standard
- Every font's complete character set
- Exact conversion procedures (that's what Course 2 will teach)

#### Activity: Build Your Quick Reference (5 minutes)

Create a simple reference document (or bookmark collection) with:

1. **Most Common Problems I'll Encounter**
   - (Based on your language context—IPA? Specific scripts? Tone marks?)

2. **Go-To Fonts**
   - Default recommendation for Latin-based linguistic work: ___________
   - For literacy materials: ___________
   - For academic publications: ___________

3. **When I'm Stuck**
   - Check encoding in: ___________ (your preferred tool)
   - Check font coverage at: https://software.sil.org/fonts/
   - Identify characters at: https://unicode.org/charts/

**This becomes your personal toolkit** that you'll expand as you gain experience.

---

## Challenge (10-12 minutes)

### The Complete Diagnostic

Now let's put everything together with a realistic scenario.

#### Scenario: The Problematic Lexicon

You receive this email:

> "Hi! I'm working on a lexicon for a language in Indonesia. The community gave me data in a Word file. When I open it:
> - Some IPA symbols show as boxes: [▯] 
> - The word 'ñ' shows as 'Ã±'
> - Tone marks like ǎ look fine in Word but break when I save as PDF
> 
> Can you help me figure out what's wrong? I'm attaching the file."

#### Your Diagnostic Process

Work through this systematically:

**Step 1: What patterns do you see?**
- Boxes → font problem
- 'Ã±' → mojibake (encoding problem)
- Breaks in PDF → rendering problem

**Multiple problems!** This file has all three culprits.

**Step 2: What would you check first?**

Start with encoding (always fix data problems before display problems):

**Actions:**
1. Open file in Notepad++ or text editor that shows encoding
2. Check: Is it UTF-8? Windows-1252? Something else?
3. If it shows 'Ã±', it's probably UTF-8 being read as Windows-1252

**Step 3: What would you check next?**

After fixing encoding, address the font:

**Actions:**
1. What font is the document using?
2. Does that font have IPA coverage?
3. Recommend switching to Charis SIL or Doulos SIL

**Step 4: What about the PDF problem?**

This is a rendering issue—Word's rendering engine handles tone marks correctly, but the PDF export doesn't.

**Actions:**
1. Check if the tone marks are precomposed or decomposed (using Unicode values)
2. Consider normalizing to NFC (precomposed) before PDF export
3. Or use a different PDF export method that preserves complex characters

**Step 5: What would you tell the colleague?**

Write a brief response explaining:
- What's wrong (three separate issues)
- What to fix first (encoding)
- What to fix next (font)
- What needs further investigation (PDF rendering)

#### Your Turn (5 minutes)

Draft a response to the colleague. Keep it:
- Clear and jargon-free (they're not a technologist)
- Actionable (specific steps they can take)
- Encouraging (they haven't broken anything permanently)

**Sample response:**

> "Good news—your data isn't lost! You have three separate issues that we can fix:
>
> 1. **Encoding mismatch** (the 'Ã±' problem): Your file is UTF-8 but being opened as Windows-1252. Try opening it in Notepad++ and explicitly choosing UTF-8 encoding. This should fix the scrambled characters.
>
> 2. **Font coverage** (the boxes): Your current font doesn't include IPA symbols. Download Charis SIL from software.sil.org/charis/ and change your document to use that font. The boxes should become proper IPA characters.
>
> 3. **PDF rendering** (the tone marks): This is trickier. Word displays them correctly, but the PDF export doesn't handle complex diacritics well. Once we fix issues 1 and 2, let's troubleshoot the PDF export together.
>
> Start with #1 and #2, then let me know what you see. We'll tackle the PDF issue after that."

---

## Change (5-8 minutes)

### Your Diagnostic Competency

You now have a complete diagnostic toolkit:

**Recognition** (Module 1A)
- Boxes, wrong marks, mojibake

**Understanding** (Module 1B)
- Encoding, fonts, rendering/combining

**Investigation** (Module 1C)
- Check encoding, check font coverage, identify characters, consult resources

This is your **"Has Knowledge"** foundation. You can:
- ✅ Identify what's wrong
- ✅ Understand why it's happening
- ✅ Investigate systematically
- ✅ Explain problems to others
- ✅ Know where to find help

### What Comes Next: Course 2

In Course 2 (Troubleshooting & Conversion), you'll move from **diagnosis** to **fixing**:
- Actually converting files between encodings
- Repairing damaged text
- Setting up systems to prevent problems
- Training others to use appropriate tools

But you can't fix what you can't diagnose—which is why this foundation matters.

### Applying This in Your Work

Before Course 2, start practicing:

**When you encounter broken text:**
1. Stop and diagnose before fixing
2. Ask: Which pattern? Which culprit?
3. Investigate systematically using your tools
4. Document what you find

**When someone asks for help:**
1. Teach them to recognize patterns
2. Show them how to investigate
3. Point them to resources
4. Build their diagnostic skills, not just fix it for them

**This is the shift from "helper" to "consultant"**—you're building capacity, not just solving immediate problems.

### Your Next Steps

**Immediate actions:**
- [ ] Bookmark the essential resources (SIL fonts, Unicode charts, etc.)
- [ ] Install a text editor that shows encoding (like Notepad++)
- [ ] Download at least one SIL font (Charis SIL recommended)
- [ ] Create your personal quick-reference guide

**Practice opportunities:**
- [ ] Look for broken text in your current work
- [ ] Practice diagnosing before fixing
- [ ] Help a colleague understand a font/encoding problem
- [ ] Build your pattern recognition with real examples

**Ready for more:**
- [ ] Complete Course 2: Troubleshooting & Conversion (when available)
- [ ] Explore SIL's other language technology tools
- [ ] Consider the "With Assistance" → "Independent" growth path

### Final Reflection

Think about this question (you don't need to answer now):

**"What font or encoding problem in your work context would you most like to be able to solve confidently?"**

Keep that in mind as you move forward. Your diagnostic skills are the foundation—now it's time to build the fixing skills on top of them.

---

**End of Module 1C**

---

**End of Course 1: Recognition & Diagnosis**
