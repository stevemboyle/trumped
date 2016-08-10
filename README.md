# The Trump Endorsement Reminder
##### A Chrome Extension to Hold Trump's Endorsers Accountable... Long After November 2016

After November 8th, 2016 (if not well before then), many politicians who have endorsed Trump will try to walk back their endorsements and wipe their hands clean of him.

Let's not let them.

The Trump Endorsement Reminder Chrome Extension places endorsement quotes as epithets in the names of politicians. For example, `Paul Ryan` becomes `Paul "we have more common ground than disagreement" Ryan`.

## Traversing the DOM

`function walk(rootNode)
{
    // Find all the text nodes in rootNode
    var walker = document.createTreeWalker(
        rootNode,
        NodeFilter.SHOW_TEXT,
        null,
        false
    ),
    node;

    while (node = walker.nextNode()) {
        handleText(node);
    }
}

function handleText(textNode) {
  textNode.nodeValue = replaceText(textNode.nodeValue);
}
`

`function observerCallback(mutations) {
    var i;

    mutations.forEach(function(mutation) {
        for (i = 0; i < mutation.addedNodes.length; i++) {
            if (mutation.addedNodes[i].nodeType === 3) {
                // Replace the text for text nodes
                handleText(mutation.addedNodes[i]);
            } else {
                // Otherwise, find text nodes within the given node and replace text
                walk(mutation.addedNodes[i]);
            }
        }
    });
}

function walkAndObserve(doc) {
    var docTitle = doc.getElementsByTagName('title')[0],
    observerConfig = {
        characterData: true,
        childList: true,
        subtree: true
    },
    bodyObserver, titleObserver;

    walk(doc.body);
    doc.title = replaceText(doc.title);

    bodyObserver = new MutationObserver(observerCallback);
    bodyObserver.observe(doc.body, observerConfig);

    if (docTitle) {
        titleObserver = new MutationObserver(observerCallback);
        titleObserver.observe(docTitle, observerConfig);
    }
}
walkAndObserve(document);`
