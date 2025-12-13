# Module 1B: The Three Culprits

**Duration:** 45-60 minutes  
**Level:** Has Knowledge  
**Goal:** Understand how encoding, fonts, and rendering work—and why they fail

---

## Connect (8-10 minutes)

### From Pattern to Problem

In Module 1A, you learned to recognize three patterns of broken text:
- **Boxes** (font missing glyphs)
- **Wrong marks** (encoding/combining character issues)
- **Mojibake** (encoding mismatch)

You can now *see* the problem. But to fix it (or help someone else fix it), you need to understand *why* it happens.

### The Detective's Question

Imagine you're helping a colleague troubleshoot a file. They say: "I changed the font and the boxes went away, but now the tone marks are in the wrong place. Did I break something?"

To answer that, you need to know:
- What did the font change actually do?
- Why didn't it fix the tone marks?
- Are these two separate problems, or related?

**This module gives you the conceptual map** to answer those questions.

### Your Experience

Before we dive in, take a moment:

**If you've ever tried to fix broken text:**
- What did you try first?
- Did it work? Why or why not?
- What did you wish you'd understood at the time?

**If you haven't encountered this yet:**
- What would you be most nervous about breaking?
- What would you want to know before you started changing things?

*(2 minutes for reflection or discussion)*

---

## Content (30-35 minutes)

### Culprit 1: Encoding (The Map)

#### What Encoding Actually Is

When you type a letter, the computer doesn't store "the letter a"—it stores a number. **Encoding is the map** that tells the computer which number represents which character.

Think of it like this:
- You type: `café`
- The computer stores: `99 97 102 233`
- The encoding says: "99 = c, 97 = a, 102 = f, 233 = é"

The problem? **Different encodings use different maps.**

In Windows-1252 encoding, the number 233 means "é"  
In UTF-8 encoding, "é" is represented by *two* numbers: `195 169`

**This is why mojibake happens.** If a file saved in UTF-8 is opened with Windows-1252, the computer reads the wrong map. It sees 195 and 169 separately and displays whatever those numbers mean in Windows-1252—which gives you "Ã©" instead of "é".

#### Legacy Encodings vs. Unicode

For decades, every language/region had its own encoding:
- Windows-1252 for Western European languages
- Windows-1251 for Cyrillic
- ISO-8859-6 for Arabic
- Dozens more

This created chaos. You couldn't mix languages in one document. Files broke when moved between systems.

**Unicode solved this** by creating one giant map that includes every character from every writing system. Instead of dozens of encodings, we now have one standard.

**UTF-8** is the most common way to store Unicode text. It's what most modern systems use.

#### But Legacy Encodings Still Exist

Many language communities collected data *before* Unicode became standard. Those files might be in:
- SIL's legacy ethnologue codes
- Custom community encodings
- Old versions of Windows encodings

**When you encounter mojibake, you're usually seeing a legacy encoding read as UTF-8, or vice versa.**

#### Regional Variations

**Note:** Windows-1252 is common for Western European languages, but other regions had their own code pages:
- Windows-1251 for Cyrillic scripts  
- Windows-1254 for Turkish  
- Windows-1256 for Arabic  
- Windows-1258 for Vietnamese  
- And many others in the 125x range

If you're working in a specific region and encounter mojibake, it's worth searching "Windows code page [your region]" to see which legacy encoding might have been used.

**Quick activity (2 minutes):** What region are you working in? Do a quick search for "Windows code page [your language/region]" and see what comes up. Bookmark any relevant results for future reference.

#### Activity: Spot the Encoding Problem (5 minutes)

Look at these three examples. Which ones are encoding problems?

**Example A:**  
File displays: `The naïve café owner`  
Should display: `The naïve café owner`

**Example B:**  
File displays: `[ŋ] is a velar nasal` with a box instead of ŋ  
Should display: `[ŋ] is a velar nasal`

**Example C:**  
File displays: `Tá»ng` (Vietnamese)  
Should display: `Tổng`

**Answers:**
- **A:** Encoding problem (mojibake—UTF-8 read as Windows-1252)
- **B:** NOT encoding—this is a font problem (the font doesn't have the ŋ glyph)
- **C:** Encoding problem (Vietnamese tone marks stored incorrectly or misread)

**Key insight:** If you see *scrambled* characters (mojibake), it's encoding. If you see *missing* characters (boxes), it's usually font.

---

### Culprit 2: Fonts (The Kitchen)

#### What Fonts Actually Contain

A font is a collection of **glyphs**—the visual shapes that represent characters.

When you install a font like Arial, you're installing a file that contains:
- The shape for "A"
- The shape for "B"
- The shape for "é"
- ...and so on for thousands of characters

**But not all fonts contain all characters.**

Arial has basic Latin letters and common European accents. It does *not* have:
- IPA symbols like [ŋ] or [ʈ] or [ɲ]
- Many non-European scripts
- Specialized linguistic characters

**This is why you see boxes.** The computer knows which character to display (the encoding is fine), but the font says "I don't have a shape for that."

#### Why SIL Fonts Matter

SIL has developed fonts specifically for linguistic work:
- **Charis SIL** (serif font, similar to Charter)
- **Doulos SIL** (serif font, similar to Times New Roman)
- **Andika** (sans-serif, designed for literacy materials)
- **Gentium Plus** (elegant serif with broad character support)

These fonts include:
- Full IPA coverage
- Extended Latin characters
- Tone marks and diacritics
- Characters for minority and endangered languages

**When you see boxes in linguistic data, switching to a SIL font often solves it immediately.**

#### Activity: Font Coverage Check (8 minutes)

Let's explore what fonts can actually do.

**Step 1:** Go to https://software.sil.org/charis/  
**Step 2:** Scroll down and look at the "Character Set Support" section  
**Step 3:** Notice how they list which Unicode blocks are covered

**Now compare:**
- Open a text editor on your computer
- Type these characters (or copy-paste them): `[ŋ ʈ ɲ ɔ̃]`
- Change the font to Arial, then Times New Roman, then (if you have it) Charis SIL

**What do you see?**
- In Arial/Times: Probably boxes for most of these
- In Charis SIL: All characters display correctly

**Key insight:** The same text, the same encoding—but different fonts show different results. The *data* is fine. The font coverage is what changes.

#### But Wait—What About Combining Characters?

Sometimes you see the base letter correctly but the accent mark is floating or misplaced. **This isn't usually a font problem**—it's an encoding issue related to how diacritics are stored.

Let's look at that next.

---

### Culprit 3: Combining Characters and Normalization

#### Two Ways to Store "é"

Unicode allows two different ways to store the same visual character:

**Method 1: Precomposed** (one number)
- `é` is stored as a single character (Unicode U+00E9)
- Called **NFC** (Normalization Form Composed)

**Method 2: Decomposed** (two numbers)
- `é` is stored as `e` (U+0065) + combining acute accent (U+0301)
- Called **NFD** (Normalization Form Decomposed)

Both are valid Unicode. Both should display the same way. But sometimes they don't.

#### When Combining Characters Go Wrong

Problems happen when:
1. **The font doesn't handle combining characters well** (poor rendering engine support)
2. **Software expects one form but receives another** (NFC vs NFD mismatch)
3. **Characters are stored in the wrong order** (combining marks applied to the wrong base)

**This is why you see floating or misplaced diacritics.**

#### Example: Tone Marks in Southeast Asian Languages

Many tone languages use stacked diacritics:
```
Intended: ṵ̂ (u with two marks: macron below + circumflex above)
What happens: The marks appear separately or stack incorrectly
```

This happens when:
- The font doesn't support complex mark positioning
- The combining characters are in the wrong order in the file
- The software doesn't normalize properly

**Key insight:** Even with the right font and right encoding, combining characters can still break if the rendering engine or normalization is wrong.

#### Activity: Spotting Combining Character Issues (5 minutes)

Which of these is likely a combining character problem?

**Example A:** `café` displays as `caf▯`  
**Example B:** `café` displays with the accent floating above the space after the e  
**Example C:** `café` displays as `cafÃ©`

**Answers:**
- **A:** Font problem (missing glyph for é)
- **B:** Combining character problem (decomposed é with wrong attachment)
- **C:** Encoding problem (mojibake)

**Implementation Note:** These examples may display correctly in modern browsers. For actual course delivery, you'll need screenshots captured from systems where the problems genuinely occur.

---

### Culprit 4: Rendering Engines (The Chef)

#### What Rendering Does

The **rendering engine** is the software that takes the encoded characters and the font glyphs and puts them together on screen.

Different programs render text differently:
- Web browsers (Chrome, Firefox, Safari)
- Word processors (Microsoft Word, LibreOffice)
- PDF readers
- Text editors

Even with the same font and encoding, **different programs might display text differently** because their rendering engines handle complex scripts differently.

#### When Rendering Fails

Rendering problems usually show up with:
- Complex scripts (Arabic, Devanagari, Thai)
- Heavy use of combining characters
- Right-to-left text mixed with left-to-right
- Ligatures and contextual forms

**For IPA and most Latin-based linguistic data, rendering problems are less common** than font or encoding issues. But they can happen.

#### Example: PDF vs. Word

You might create a document in Word with perfect IPA display, then export to PDF and see problems. This isn't because the data changed—it's because the PDF rendering engine handles the font differently than Word did.

**Key insight:** Same data, same font—but different rendering = different display.

---

## Challenge (10-12 minutes)

### The Diagnostic Challenge

You're going to practice diagnosing problems by asking the right questions.

#### Scenario 1: The Disappearing Glottals

A colleague sends you a file with these words from a language in Ethiopia:
```
ʔəṭəna (should be: ʔəṭəna)
ʃəṭəm (should be: ʃəṭəm)
```

The glottal stop (ʔ) and the pharyngeal fricative (ʃ) both display as boxes.

**Your diagnostic questions:**
1. What pattern is this? (Box, wrong mark, mojibake?)
2. Which culprit is most likely? (Encoding, font, rendering?)
3. What would you check first?

**Answers:**
1. Box pattern
2. Font problem (most likely the font doesn't have these IPA characters)
3. Check if they're using a font with IPA coverage—suggest switching to Charis SIL or Doulos SIL

**Implementation Note:** Capture actual screenshots showing boxes, as these IPA characters may render correctly in learners' browsers depending on their system fonts.

---

#### Scenario 2: The Wandering Tone Marks

A lexicographer working with a tonal language shows you this:
```
Typed: tɔ̃́ː (with nasalization and high tone)
Displays: tɔ ́̃ː (with marks floating separately)
```

**Your diagnostic questions:**
1. What pattern is this?
2. Which culprit is most likely?
3. What would you investigate?

**Answers:**
1. Wrong mark pattern (marks in wrong place)
2. Could be combining character issue (NFD vs NFC) OR font rendering problem
3. Check: (a) Is the font designed for complex diacritics? (b) Are the combining marks in the right order? (c) Try a different font known for good mark positioning (like Charis SIL)

---

#### Scenario 3: The Email That Broke

Someone forwards you an email. When they composed it, everything looked fine. When you receive it, you see:
```
The linguistâ€™s data shows interesting patterns
```

Should show: `The linguist's data shows interesting patterns`

**Your diagnostic questions:**
1. What pattern is this?
2. Which culprit is most likely?
3. What happened?

**Answers:**
1. Mojibake pattern (scrambled characters)
2. Encoding mismatch
3. The email was composed in UTF-8 but read as Windows-1252 (or vice versa). The typographic apostrophe (') got mangled in transit.

---

### Discussion: Multiple Culprits

Sometimes a file has *multiple* problems:
- Wrong encoding (mojibake) AND missing font glyphs (boxes)
- Combining character issues AND rendering problems

**When you see multiple problems, fix them in order:**
1. Fix encoding first (mojibake)
2. Then fix font issues (boxes)
3. Then address combining character problems (wrong marks)

Why this order? Because **you can't diagnose font problems if the encoding is wrong.** Get the data readable first, then make it display correctly.

---

## Change (5 minutes)

### Your Conceptual Map

You now understand the three culprits behind broken text:

1. **Encoding** = the map from numbers to characters (mojibake when wrong)
2. **Fonts** = the shapes to draw (boxes when glyphs missing)
3. **Rendering/Combining** = putting it together (wrong marks when broken)

### What This Means for Your Work

Next time you encounter broken text, you won't just see the problem—you'll understand *why* it's happening.

This understanding is what separates someone who can follow a recipe ("try this font") from someone who can **diagnose and solve new problems** they've never seen before.

**That's transfer.** That's what makes you valuable as a language technologist.

### Before Module 1C

**Bookmark these resources:**
- https://software.sil.org/fonts/ (SIL fonts with linguistic character support)
- https://unicode.org/charts/ (Unicode character charts—useful for identifying characters)

**Optional practice:**
- Find a file with broken text (or create one by changing encoding)
- Try to diagnose: Which culprit is it?
- Don't fix it yet—just practice identifying the problem

### Looking Ahead

In **Module 1C: Your Diagnostic Toolkit**, we'll pull everything together. You'll learn:
- How to inspect files to see their actual encoding
- How to check which characters a font supports
- How to document problems clearly so others (or Course 2) can fix them

You're building toward **diagnostic competence**—the foundation for everything that comes next.

---

**End of Module 1B**
