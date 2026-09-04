# DOCX Code Templates

Technical code patterns for creating IGCSE Global Perspectives exam documents using docx-js.

---

## Critical Settings

### Page Setup
```javascript
const PAGE_WIDTH = 12240;  // US Letter width in DXA (8.5")
const PAGE_HEIGHT = 15840; // US Letter height in DXA (11")
const MARGIN = 1440;       // 1 inch margins in DXA
```

### Font Defaults
```javascript
const DEFAULT_FONT = "Arial";
const DEFAULT_SIZE = 24;  // 12pt (size in half-points)
const HEADING_SIZE = 28;  // 14pt
const TITLE_SIZE = 32;    // 16pt
```

---

## Common Issues and Solutions

### ❌ NEVER Use Unicode Bullets
```javascript
// WRONG - will render incorrectly
new Paragraph({
    children: [new TextRun("• Item text")]
})

// CORRECT - use proper bullet formatting
new Paragraph({
    bullet: { level: 0 },
    children: [new TextRun("Item text")]
})
```

### ❌ NEVER Use ShadingType.SOLID for Tables
```javascript
// WRONG - causes grey background issues
new TableCell({
    shading: { type: ShadingType.SOLID, color: "FFFFFF" }
})

// CORRECT - use CLEAR for white background
new TableCell({
    shading: { type: ShadingType.CLEAR, color: "auto", fill: "FFFFFF" }
})
```

### Bullet List Configuration
```javascript
const doc = new Document({
    numbering: {
        config: [{
            reference: "bullet-list",
            levels: [{
                level: 0,
                format: LevelFormat.BULLET,
                text: "•",
                alignment: AlignmentType.LEFT,
                style: {
                    paragraph: {
                        indent: { left: 720, hanging: 360 }
                    }
                }
            }]
        }]
    }
});

// Using the bullet list
new Paragraph({
    numbering: { reference: "bullet-list", level: 0 },
    children: [new TextRun("Bullet item")]
})
```

---

## Document Templates

### Question Paper Structure
```javascript
const doc = new Document({
    sections: [{
        properties: {
            page: {
                size: { width: PAGE_WIDTH, height: PAGE_HEIGHT },
                margin: {
                    top: MARGIN, bottom: MARGIN,
                    left: MARGIN, right: MARGIN
                }
            }
        },
        children: [
            // Cover page elements
            createCoverPage(),
            // Page break
            new Paragraph({ children: [new PageBreak()] }),
            // Questions
            createQuestion1(),
            createQuestion2(),
            createQuestion3(),
            createQuestion4()
        ]
    }]
});
```

### Cover Page Template
```javascript
function createCoverPage() {
    return [
        new Paragraph({
            alignment: AlignmentType.CENTER,
            children: [
                new TextRun({
                    text: "Cambridge IGCSE™",
                    font: DEFAULT_FONT,
                    size: TITLE_SIZE,
                    bold: true
                })
            ]
        }),
        new Paragraph({
            alignment: AlignmentType.CENTER,
            spacing: { before: 400 },
            children: [
                new TextRun({
                    text: "GLOBAL PERSPECTIVES",
                    font: DEFAULT_FONT,
                    size: HEADING_SIZE,
                    bold: true
                }),
                new TextRun({
                    text: "                                              0457/01",
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        }),
        // ... additional cover page elements
    ];
}
```

---

## Source Booklet (Insert) Template

### Source Header
```javascript
function createSourceHeader(sourceNumber, sourceTitle) {
    return new Paragraph({
        spacing: { before: 400, after: 200 },
        children: [
            new TextRun({
                text: `Source ${sourceNumber}: `,
                font: DEFAULT_FONT,
                size: DEFAULT_SIZE,
                bold: true
            }),
            new TextRun({
                text: sourceTitle,
                font: DEFAULT_FONT,
                size: DEFAULT_SIZE,
                bold: true
            })
        ]
    });
}
```

### Source Attribution
```javascript
function createSourceAttribution(attribution) {
    return new Paragraph({
        spacing: { before: 100 },
        children: [
            new TextRun({
                text: attribution,
                font: DEFAULT_FONT,
                size: 20,  // 10pt
                italics: true
            })
        ]
    });
}
```

### Source 4 - Debate Format
```javascript
function createDebateSource(speaker1, text1, speaker2, text2) {
    return [
        new Paragraph({
            spacing: { before: 200 },
            children: [
                new TextRun({
                    text: `${speaker1}:`,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true
                })
            ]
        }),
        new Paragraph({
            spacing: { before: 100 },
            children: [
                new TextRun({
                    text: text1,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        }),
        new Paragraph({
            spacing: { before: 300 },
            children: [
                new TextRun({
                    text: `${speaker2}:`,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true
                })
            ]
        }),
        new Paragraph({
            spacing: { before: 100 },
            children: [
                new TextRun({
                    text: text2,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        })
    ];
}
```

---

## Question Paper Templates

### Question with Sub-parts
```javascript
function createQuestion1(questionData) {
    return [
        // Main question header
        new Paragraph({
            spacing: { before: 400 },
            children: [
                new TextRun({
                    text: "1   Study Sources 1 and 2.",
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true
                })
            ]
        }),

        // Part (a)
        new Paragraph({
            spacing: { before: 300 },
            indent: { left: 720 },
            children: [
                new TextRun({
                    text: "(a) ",
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true
                }),
                new TextRun({
                    text: questionData.partA,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        }),

        // Answer lines
        createAnswerLines(2),

        // Mark allocation (right-aligned)
        new Paragraph({
            alignment: AlignmentType.RIGHT,
            children: [
                new TextRun({
                    text: "[1]",
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        })
    ];
}
```

### Answer Lines
```javascript
function createAnswerLines(count) {
    const lines = [];
    for (let i = 0; i < count; i++) {
        lines.push(
            new Paragraph({
                spacing: { before: 200 },
                indent: { left: 720 },
                children: [
                    new TextRun({
                        text: ".........................................................................",
                        font: DEFAULT_FONT,
                        size: DEFAULT_SIZE
                    })
                ]
            })
        );
    }
    return lines;
}
```

### Bullet Points in Questions
```javascript
function createQuestionBullets(bullets) {
    return bullets.map(bulletText =>
        new Paragraph({
            numbering: { reference: "bullet-list", level: 0 },
            indent: { left: 1080 },
            children: [
                new TextRun({
                    text: bulletText,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        })
    );
}

// Usage for Q3
createQuestionBullets([
    "consider both arguments",
    "evaluate their reasoning, evidence and use of language",
    "support your judgement with their words and ideas."
]);
```

---

## Mark Scheme Templates

### Level Descriptor Table
```javascript
function createLevelTable(levels) {
    return new Table({
        width: { size: 100, type: WidthType.PERCENTAGE },
        rows: [
            // Header row
            new TableRow({
                children: [
                    createTableCell("Level", true),
                    createTableCell("Marks", true),
                    createTableCell("Descriptor", true)
                ]
            }),
            // Level rows
            ...levels.map(level =>
                new TableRow({
                    children: [
                        createTableCell(level.level.toString()),
                        createTableCell(level.marks),
                        createTableCell(level.descriptor)
                    ]
                })
            )
        ]
    });
}

function createTableCell(text, isHeader = false) {
    return new TableCell({
        shading: {
            type: ShadingType.CLEAR,
            color: "auto",
            fill: isHeader ? "D9D9D9" : "FFFFFF"
        },
        children: [
            new Paragraph({
                children: [
                    new TextRun({
                        text: text,
                        font: DEFAULT_FONT,
                        size: DEFAULT_SIZE,
                        bold: isHeader
                    })
                ]
            })
        ]
    });
}
```

### Indicative Content
```javascript
function createIndicativeContent(title, bullets) {
    return [
        new Paragraph({
            spacing: { before: 200 },
            children: [
                new TextRun({
                    text: title,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true
                })
            ]
        }),
        ...bullets.map(bullet =>
            new Paragraph({
                numbering: { reference: "bullet-list", level: 0 },
                children: [
                    new TextRun({
                        text: bullet,
                        font: DEFAULT_FONT,
                        size: DEFAULT_SIZE
                    })
                ]
            })
        )
    ];
}
```

---

## Exemplar Answer Templates

### Exemplar with Commentary
```javascript
function createExemplar(questionPart, answer, commentary) {
    return [
        // Question reference
        new Paragraph({
            spacing: { before: 400 },
            children: [
                new TextRun({
                    text: `Question ${questionPart}`,
                    font: DEFAULT_FONT,
                    size: HEADING_SIZE,
                    bold: true
                })
            ]
        }),

        // Exemplar answer
        new Paragraph({
            spacing: { before: 200 },
            shading: { type: ShadingType.CLEAR, fill: "F0F0F0" },
            children: [
                new TextRun({
                    text: "Exemplar Answer:",
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true
                })
            ]
        }),
        new Paragraph({
            shading: { type: ShadingType.CLEAR, fill: "F0F0F0" },
            children: [
                new TextRun({
                    text: answer,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE
                })
            ]
        }),

        // Commentary
        new Paragraph({
            spacing: { before: 200 },
            children: [
                new TextRun({
                    text: "Why this is a strong response:",
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    bold: true,
                    italics: true
                })
            ]
        }),
        new Paragraph({
            children: [
                new TextRun({
                    text: commentary,
                    font: DEFAULT_FONT,
                    size: DEFAULT_SIZE,
                    italics: true
                })
            ]
        })
    ];
}
```

---

## Student Writing Support Templates

### Sentence Stems Box
```javascript
function createSentenceStemsBox(title, stems) {
    return new Table({
        width: { size: 100, type: WidthType.PERCENTAGE },
        borders: {
            top: { style: BorderStyle.SINGLE, size: 1 },
            bottom: { style: BorderStyle.SINGLE, size: 1 },
            left: { style: BorderStyle.SINGLE, size: 1 },
            right: { style: BorderStyle.SINGLE, size: 1 }
        },
        rows: [
            new TableRow({
                children: [
                    new TableCell({
                        shading: { type: ShadingType.CLEAR, fill: "E6F3FF" },
                        children: [
                            new Paragraph({
                                children: [
                                    new TextRun({
                                        text: title,
                                        font: DEFAULT_FONT,
                                        size: DEFAULT_SIZE,
                                        bold: true
                                    })
                                ]
                            }),
                            ...stems.map(stem =>
                                new Paragraph({
                                    numbering: { reference: "bullet-list", level: 0 },
                                    children: [
                                        new TextRun({
                                            text: stem,
                                            font: DEFAULT_FONT,
                                            size: DEFAULT_SIZE
                                        })
                                    ]
                                })
                            )
                        ]
                    })
                ]
            })
        ]
    });
}
```

---

## File Generation

### Save Document
```javascript
async function saveDocument(doc, filename) {
    const buffer = await Packer.toBuffer(doc);
    fs.writeFileSync(filename, buffer);
}

// Usage
const doc = new Document({ /* ... */ });
await saveDocument(doc, "Mock_1_GP_Question_Paper.docx");
```

### Complete Document Example
```javascript
const { Document, Packer, Paragraph, TextRun, PageBreak,
        Table, TableRow, TableCell, BorderStyle,
        AlignmentType, ShadingType, WidthType,
        LevelFormat } = require("docx");
const fs = require("fs");

async function createQuestionPaper(examData) {
    const doc = new Document({
        numbering: {
            config: [{
                reference: "bullet-list",
                levels: [{
                    level: 0,
                    format: LevelFormat.BULLET,
                    text: "•",
                    alignment: AlignmentType.LEFT,
                    style: {
                        paragraph: {
                            indent: { left: 720, hanging: 360 }
                        }
                    }
                }]
            }]
        },
        sections: [{
            properties: {
                page: {
                    size: { width: 12240, height: 15840 },
                    margin: { top: 1440, bottom: 1440, left: 1440, right: 1440 }
                }
            },
            children: [
                ...createCoverPage(examData),
                new Paragraph({ children: [new PageBreak()] }),
                ...createQuestion1(examData.q1),
                ...createQuestion2(examData.q2),
                ...createQuestion3(examData.q3),
                ...createQuestion4(examData.q4)
            ]
        }]
    });

    await saveDocument(doc, examData.filename);
}
```

---

## Checklist Before Generation

- [ ] Page size: US Letter (12240 x 15840 DXA)
- [ ] Margins: 1 inch (1440 DXA)
- [ ] Font: Arial throughout
- [ ] Bullets: Using LevelFormat.BULLET, not unicode
- [ ] Table shading: Using ShadingType.CLEAR, not SOLID
- [ ] Spacing: Consistent before/after paragraphs
- [ ] Mark allocations: Right-aligned in brackets
- [ ] Page breaks: Between major sections

---

*Reference document for IGCSE GP Exam Creator v3.0*
