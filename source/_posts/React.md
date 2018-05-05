---
title: React
date: 2018-05-05 09:19:35
tags: React
---

> what different between Reactv15 and Reactv16

Since v16, React use Fiber structure to update React componet, that process is asynchronous instead of synchronous in v15.

Update process in Fiber has two phrase
1.  reconciliation phrase 
2.  commit phrase 

In reconciliation phrase, React find DOM needed to be updated. This phrase cant be interrupted if React found there are some more priority task needed to be executed.

In commit phrase, React update the DOM and wouldn't be interrupted.

In reconciliation phrase, these component recycle will be called, so they are be affected. Ther will be called several times properly.
* componentWillMount (caution: avoid execute login much times and you just want to execute one time)
* componentWillUpdate (caution)
* componentWillReceiveProps (called when parent component props update. no big matter)
* shouldComponentUpdate (just return true or false. no big matter)

In commit
* componentDidMount
* componentDidUpdate
* componentWillUnmount

