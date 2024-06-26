# Building

- getRooms
	- if this.rooms != null
		- return this.rooms
	- this.zip.file(this.buildingFile)
	- getFileFromZip(zip, this.buildingFile);

for all cells in a buildingCellArr
	does it have a class attribute?
		if yes, does it match one of our required fields?
			if yes, populate that field
		else, continue
	else continue

if the fields are not all populated, return null or error?

if this node has classes, retrieve the corresponding value
BuildingTable:
```json
{
  nodeName: 'table',
  tagName: 'table',
  attrs: [ { name: 'class', value: 'views-table cols-5 table' } ],
  namespaceURI: 'http://www.w3.org/1999/xhtml',
  childNodes: [
    {
      nodeName: '#text',
      value: '\n         ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'thead',
      tagName: 'thead',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    { nodeName: '#text', value: '\n    ', parentNode: [Circular *1] },
    {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    { nodeName: '#text', value: '\n', parentNode: [Circular *1] }
  ],
  parentNode: {
    nodeName: 'div',
    tagName: 'div',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [ [Object], [Circular *1], [Object] ],
    parentNode: {
      nodeName: 'div',
      tagName: 'div',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  }
}
```

buildingTableBody:
```json
{
  nodeName: 'tbody',
  tagName: 'tbody',
  attrs: [],
  namespaceURI: 'http://www.w3.org/1999/xhtml',
  childNodes: [
    {
      nodeName: '#text',
      value: '\n          ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'tr',
      tagName: 'tr',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n          ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'tr',
      tagName: 'tr',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n          ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'tr',
      tagName: 'tr',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n          ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'tr',
      tagName: 'tr',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n          ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'tr',
      tagName: 'tr',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n          ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'tr',
      tagName: 'tr',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    { nodeName: '#text', value: '\n      ', parentNode: [Circular *1] }
  ],
  parentNode: {
    nodeName: 'table',
    tagName: 'table',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [ [Object], [Object], [Object], [Circular *1], [Object] ],
    parentNode: {
      nodeName: 'div',
      tagName: 'div',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  }
}

```

BuildingRows:
```json
[
  {
    nodeName: 'tr',
    tagName: 'tr',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  },
  {
    nodeName: 'tr',
    tagName: 'tr',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  },
  {
    nodeName: 'tr',
    tagName: 'tr',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  },
  {
    nodeName: 'tr',
    tagName: 'tr',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  },
  {
    nodeName: 'tr',
    tagName: 'tr',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  },
  {
    nodeName: 'tr',
    tagName: 'tr',
    attrs: [ [Object] ],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'tbody',
      tagName: 'tbody',
      attrs: [],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  }
]
```

BuldingRow:
```json
{
  nodeName: 'tr',
  tagName: 'tr',
  attrs: [ { name: 'class', value: 'odd views-row-first' } ],
  namespaceURI: 'http://www.w3.org/1999/xhtml',
  childNodes: [
    {
      nodeName: '#text',
      value: '\n                  ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'td',
      tagName: 'td',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n                  ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'td',
      tagName: 'td',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n                  ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'td',
      tagName: 'td',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n                  ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'td',
      tagName: 'td',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n                  ',
      parentNode: [Circular *1]
    },
    {
      nodeName: 'td',
      tagName: 'td',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Circular *1]
    },
    {
      nodeName: '#text',
      value: '\n              ',
      parentNode: [Circular *1]
    }
  ],
  parentNode: {
    nodeName: 'tbody',
    tagName: 'tbody',
    attrs: [],
    namespaceURI: 'http://www.w3.org/1999/xhtml',
    childNodes: [
      [Object], [Circular *1],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    parentNode: {
      nodeName: 'table',
      tagName: 'table',
      attrs: [Array],
      namespaceURI: 'http://www.w3.org/1999/xhtml',
      childNodes: [Array],
      parentNode: [Object]
    }
  }
}
```


0: image
1: code (shortname)
	in child text node
2: title (fullname)
	in child A node
		in child text node
3: address
	in child text node
4: href
	in child A node
		in attributes