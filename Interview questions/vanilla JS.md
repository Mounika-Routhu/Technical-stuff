## document fragment
1. This is a light weight in-memory container
2. useful to reduce no. of DOM changes during loops

```JS
  const optionListFragment = document.createDocumentFragment();
  optionList.forEach((op) => optionListFragment.appendChild(op));
  selectEl.appendChild(optionListFragment);
```
