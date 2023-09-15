### 一、Node: nodeType

只读属性 **`Node.nodeType`** 表示的是该节点的类型。用来帮助人们区分不同的节点类型。在Document Object Model中有下列的nodeType

| 常量                               | 值   | 描述                                                         |
| :--------------------------------- | :--- | :----------------------------------------------------------- |
| `Node.ELEMENT_NODE`                | `1`  | An Element node like <p> or <div>.                           |
| `Node.ATTRIBUTE_NODE`              | 2    | An Attribute of an Element.                                  |
| `Node.TEXT_NODE`                   | `3`  | The actual Text inside an Element or Attr.                   |
| `Node.CDATA_SECTION_NODE`          | `4`  | A CDATASection, such as <!CDATA[[ … ]]> 。ps:==在 XML 中，CDATA 可以直接包含未经转义的文本。比如 `<` 和 `&`，只要位于 CDATA 片段中，它们就不需要被转义，保持原样就可以了。== |
| `Node.PROCESSING_INSTRUCTION_NODE` | `7`  | 一个用于 XML 文档的 [`ProcessingInstruction` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/ProcessingInstruction) ，例如 `<?xml-stylesheet ... ?>` 声明。 |
| `Node.COMMENT_NODE`                | `8`  | A [`Comment`](https://developer.mozilla.org/en-US/docs/Web/API/Comment) node, such as `<!-- … -->`. |
| `Node.DOCUMENT_NODE`               | `9`  | A [`Document`](https://developer.mozilla.org/en-US/docs/Web/API/Document) node. |
| `Node.DOCUMENT_TYPE_NODE`          | `10` | 描述文档类型的 [`DocumentType`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentType) 节点。例如 `<!DOCTYPE html>` 就是用于 HTML5 的。 |
| `Node.DOCUMENT_FRAGMENT_NODE`      | `11` | A [`DocumentFragment`](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment) node. |
|                                    |      |                                                              |

```js
var type = node.nodeType;
这里的type返回的是数值


var p = document.createElement("p");
p.textContent = "很久很久以前...";

console.log(p.nodeType)  //输出为1


```
