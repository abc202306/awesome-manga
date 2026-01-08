
# script-game-moc-builder

```js
function getImageElem(wikilink) {
	const linktext = /^\[\[(.*)(\\?\|.*)?\]\]$/.exec(wikilink)[1];
	const file = app.metadataCache.getFirstLinkpathDest(linktext);
	return `<img src="${file.path}" width=200>`;
}
function getMangaItemSection(file) {
	const title = /^manga-item-(.*)/.exec(file.basename)[1];
	const fileCache = app.metadataCache.getFileCache(file);
	const fm = fileCache.frontmatter;
	
	return `### ${title}\n\n> see: [${fm['title-wikipedia']}](${fm['url-wikipedia']})\n\n${fm['description-wikipedia']}\n\n${getImageElem(fm['cover'])}\n\n${fm['cover-url']}`
}
function getMangaTagSection(file) {
	const title = /^manga-tag-(.*)/.exec(file.basename)[1];
	const fileCache = app.metadataCache.getFileCache(file);
	const fm = fileCache.frontmatter;
	
	const ol = fm['relatedpages'].map(link=>{
		const mangaitemsectionid = /^\[\[manga-item-([^\|]*)/.exec(link)[1];
		return `1. [${mangaitemsectionid}](#${mangaitemsectionid})`
	}).join("\n");
	
	return `### ${title}\n\n${ol}`
}
const mdfiles = app.vault.getMarkdownFiles();
const mangaitems = mdfiles.filter(f=>f.path.startsWith("manga-items/")).sort();
const mangatags = mdfiles.filter(f=>f.path.startsWith("manga-tag/")).sort();
console.log(`## manga-items\n\n${mangaitems.map(f=>getMangaItemSection(f)).join("\n\n")}\n\n## keyword-index\n\n${mangatags.map(f=>getMangaTagSection(f)).join("\n\n")}\n`)
```
