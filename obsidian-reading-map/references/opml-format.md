# OPML Format And Validation

## Required OPML Shape

Use OPML 2.0:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<opml version="2.0">
  <head>
    <title>书名｜读后学习复盘思维导图</title>
    <dateCreated>Fri, 03 Jul 2026 00:00:00 GMT</dateCreated>
  </head>
  <body>
    <outline text="书名｜读后学习复盘思维导图">
      <outline text="0. 一句话总纲｜...">
        <outline text="二级节点">
          <outline text="三级节点" />
        </outline>
      </outline>
    </outline>
  </body>
</opml>
```

## Generation Rules

- Use Python standard library only for OPML generation: `xml.etree.ElementTree`, `pathlib`, `re`, `argparse` if needed.
- Escape XML through the XML library, not manual replacements.
- Include `dateCreated` in GMT format.
- Do not require network access or non-standard dependencies for OPML.
- Write the OPML in UTF-8.

## Validation Checklist

Before final response:

1. Parse the generated OPML with `xml.etree.ElementTree.parse`.
2. Confirm there is exactly one root outline under `body`.
3. Count all `outline` nodes.
4. Count first-level branches under the central topic.
5. Search the OPML for banned or suspicious material:
   - External books mentioned in annotations but not part of the target book's argument.
   - `补充理解` unless deliberately allowed.
   - Generic frameworks not supported by the user's notes or target book.
6. If an online XMind map is created, create it from the same final outline, not from a stale previous version.

## Evidence Preference

When checking whether a node belongs in the map, prefer evidence in this order:

1. User notes or comments in the annotation file.
2. User highlights, especially repeated or long highlights.
3. Context field around the highlight.
4. The target book's widely recognized original framework.
5. A clearly labeled action item based on the target book's highlighted method.

If evidence is weak, omit the node.
