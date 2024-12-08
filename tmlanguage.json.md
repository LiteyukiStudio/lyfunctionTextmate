```json
{
    "$schema": "http://json-schema.org/schema#",
    "id": "https://raw.githubusercontent.com/martinring/tmlanguage/master/tmlanguage.json",
    "$ref": "#/definitions/root",
    "definitions": {
      "root": {
        "allOf": [
          { "$ref": "#/definitions/grammar" },
          {
            "type": "object",
            "properties": {
              "name": { "type": "string" },
              "scopeName": {
                "description": "这应该是语法的唯一名称，遵循点分隔名称的约定，其中每个新（最左侧）部分都专门表示名称。通常，它将是一个由两部分组成的名称，其中第一个是文本或源，第二个是语言或文档类型的名称。但是，如果要专门化现有类型，则可能需要从要专用的类型派生名称。例如，Markdown 是 text.html.markdown，Ruby on Rails（rhtml 文件）是 text.html.rails。从（在这种情况下）text.html 派生它的好处是，在 text.html 范围内工作的所有内容也将在 text.html 中工作。something» 范围（但其优先级低于专门针对 text.html 的某事»).",
                "type": "string",
                "pattern": "^(text|source)(\\.[\\w0-9-]+)+$"
              },
              "foldingStartMarker": {
                "description": "行（在文档中）匹配的正则表达式。如果一行与其中一个模式匹配（但不是两个模式），则它将成为折叠标记",
                "type": "string"
              },
              "foldingStopMarker": {
                "description": "行（在文档中）匹配的正则表达式。如果一行与其中一个模式匹配（但不是两个模式），它将成为折叠标记",
                "type": "string"
              },
              "fileTypes": {
                "description": "这是 Grammar 应该（默认情况下）一起使用的文件类型扩展名数组。当 TextMate 不知道用户打开的文件使用什么语法时，将引用此消息。但是，如果用户从状态栏中的语言弹出窗口中选择语法，TextMate 将记住该选择",
                "type": "array",
                "items": { "type": "string" }
              },
              "uuid": { "type": "string" },
              "firstLineMatch": { "type": "string" }
            },
            "required": [ "scopeName" ]
          }
        ]
      },
      "grammar": {
        "type": "object",
          "properties": {
            "patterns": {
              "type": "array",
              "items": { "$ref": "#/definitions/pattern" },
              "default": [ ]
            },
            "repository": {
              "description": "a dictionary (i.e. key/value pairs) of rules which can be included from other places in the grammar. The key is the name of the rule and the value is the actual rule. Further explanation (and example) follow with the description of the include rule key.",
              "type": "object",
              "additionalProperties": {
                "$ref": "#/definitions/pattern"
              }
            }
          },
          "required": [
            "patterns"
          ]
      },
      "captures": {
        "type": "object",
        "patternProperties": {
          "^[0-9]+$": {
            "type": "object",
            "properties": {
              "name": { "$ref": "#/definitions/name" },
              "patterns": {
                "type": "array",
                "items": { "$ref": "#/definitions/pattern" },
                "default": [ ]
              }
            },
            "additionalProperties": false
          }
        },
        "additionalProperties": false
      },
      "pattern": {
        "type": "object",
        "properties": {
          "comment": { "type": "string" },
          "disabled": { "type": "integer", "minimum": 0, "maximum": 1, "description": "set this property to 1 to disable the current pattern" },
          "include": {
            "description": "这允许您引用不同的语言，递归引用语法本身或在此文件的存储库中声明的规则。",
            "type": "string"
          },
          "match": {
            "description": "一个正则表达式，用于标识应将名称分配到的文本部分。例: '\\b(true|false)\\b'.",
            "type": "string"
          },
          "name": {
            "description": "分配给匹配部分的名称。这用于样式和特定于范围的设置和操作，这意味着它通常应从标准名称之一派生.",
            "$ref": "#/definitions/name"
          },
          "contentName": {
            "description": "this key is similar to the name key but only assigns the name to the text between what is matched by the begin/end patterns.",
            "$ref": "#/definitions/name"
          },
          "begin": {
            "description": "these keys allow matches which span several lines and must both be mutually exclusive with the match key. Each is a regular expression pattern. begin is the pattern that starts the block and end is the pattern which ends the block. Captures from the begin pattern can be referenced in the end pattern by using normal regular expression back-references. This is often used with here-docs. A begin/end rule can have nested patterns using the patterns key.",
            "type": "string"
          },
          "end": {
            "description": "these keys allow matches which span several lines and must both be mutually exclusive with the match key. Each is a regular expression pattern. begin is the pattern that starts the block and end is the pattern which ends the block. Captures from the begin pattern can be referenced in the end pattern by using normal regular expression back-references. This is often used with here-docs. A begin/end rule can have nested patterns using the patterns key.",
            "type": "string"
          },
          "while": {
            "description": "these keys allow matches which span several lines and must both be mutually exclusive with the match key. Each is a regular expression pattern. begin is the pattern that starts the block and while continues it.",
            "type": "string"
          },
          "captures": {
            "description": "allows you to assign attributes to the captures of the match pattern. Using the captures key for a begin/end rule is short-hand for giving both beginCaptures and endCaptures with same values.",
            "$ref": "#/definitions/captures"
          },
          "beginCaptures": {
            "description": "allows you to assign attributes to the captures of the begin pattern. Using the captures key for a begin/end rule is short-hand for giving both beginCaptures and endCaptures with same values.",
            "$ref": "#/definitions/captures"
          },
          "endCaptures": {
            "description": "allows you to assign attributes to the captures of the end pattern. Using the captures key for a begin/end rule is short-hand for giving both beginCaptures and endCaptures with same values.",
            "$ref": "#/definitions/captures"
          },
          "whileCaptures": {
            "description": "allows you to assign attributes to the captures of the while pattern. Using the captures key for a begin/while rule is short-hand for giving both beginCaptures and whileCaptures with same values.",
            "$ref": "#/definitions/captures"
          },
          "patterns": {
            "description": "applies to the region between the begin and end matches",
            "type": "array",
            "items": { "$ref": "#/definitions/pattern" },
            "default": [ ]
          },
          "applyEndPatternLast": {
            "type": "integer",
            "minimum": 0,
            "maximum": 1
          }
        },
        "dependencies": {
          "begin": {
            "anyOf": [{ "required": [ "end" ] }, { "required": [ "while" ] }]
          },
          "end": [ "begin" ],
          "while": [ "begin" ],
          "contentName": {
            "anyOf": [
              { "required": [ "begin", "end" ] },
              { "required": [ "begin", "while" ] }
            ]
          },
          "beginCaptures": {
            "anyOf": [
              { "required": [ "begin", "end" ] },
              { "required": [ "begin", "while" ] }
            ]
          },
          "whileCaptures": [ "begin", "while" ],
          "endCaptures": [ "begin", "end" ],
          "applyEndPatternLast": [ "end" ]
        }
      },
      "name": {
        "anyOf": [
          {
            "type": "string"
          },
          {
            "type": "string",
            "enum": [
              "comment",
              "comment.block",
              "comment.block.documentation",
              "comment.line",
              "comment.line.double-dash",
              "comment.line.double-slash",
              "comment.line.number-sign",
              "comment.line.percentage",
              "constant",
              "constant.character",
              "constant.character.escape",
              "constant.language",
              "constant.numeric",
              "constant.other",
              "constant.regexp",
              "constant.rgb-value",
              "constant.sha.git-rebase",
              "emphasis",
              "entity",
              "entity.name",
              "entity.name.class",
              "entity.name.function",
              "entity.name.method",
              "entity.name.section",
              "entity.name.selector",
              "entity.name.tag",
              "entity.name.type",
              "entity.other",
              "entity.other.attribute-name",
              "entity.other.inherited-class",
              "header",
              "invalid",
              "invalid.deprecated",
              "invalid.illegal",
              "keyword",
              "keyword.control",
              "keyword.control.less",
              "keyword.operator",
              "keyword.operator.new",
              "keyword.other",
              "keyword.other.unit",
              "markup",
              "markup.bold",
              "markup.changed",
              "markup.deleted",
              "markup.heading",
              "markup.inline.raw",
              "markup.inserted",
              "markup.italic",
              "markup.list",
              "markup.list.numbered",
              "markup.list.unnumbered",
              "markup.other",
              "markup.punctuation.list.beginning",
              "markup.punctuation.quote.beginning",
              "markup.quote",
              "markup.raw",
              "markup.underline",
              "markup.underline.link",
              "meta",
              "meta.cast",
              "meta.parameter.type.variable",
              "meta.preprocessor",
              "meta.preprocessor.numeric",
              "meta.preprocessor.string",
              "meta.return-type",
              "meta.selector",
              "meta.structure.dictionary.key.python",
              "meta.tag",
              "meta.type.annotation",
              "meta.type.name",
              "metatag.php",
              "storage",
              "storage.modifier",
              "storage.modifier.import.java",
              "storage.modifier.package.java",
              "storage.type",
              "storage.type.cs",
              "storage.type.java",
              "string",
              "string.html",
              "string.interpolated",
              "string.jade",
              "string.other",
              "string.quoted",
              "string.quoted.double",
              "string.quoted.other",
              "string.quoted.single",
              "string.quoted.triple",
              "string.regexp",
              "string.unquoted",
              "string.xml",
              "string.yaml",
              "strong",
              "support",
              "support.class",
              "support.constant",
              "support.function",
              "support.function.git-rebase",
              "support.other",
              "support.property-value",
              "support.type",
              "support.type.property-name",
              "support.type.property-name.css",
              "support.type.property-name.less",
              "support.type.property-name.scss",
              "support.variable",
              "variable",
              "variable.language",
              "variable.name",
              "variable.other",
              "variable.parameter"
            ]
          }
        ]
      }
    }
  }
```