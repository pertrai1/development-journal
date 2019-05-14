# These ARE the Angular tips you are looking for

### John Papa | ng-conf | 2019 | [YouTube](https://youtu.be/2ZFgcTOcnUg)



#### How do you manage multiple observable subscriptions?

**Q** What happens if you don't unsubscribe from a subscription?

**A** You have memory leaks

**Q** How do you manage unsubscribing from subscriptions?

**A** You can manually do so on the `ngOnDestroy` after you have created a variable to hold the subscription. Only problem that happens here is that you could have numerous subscriptions, which will then cause you to have to manually handle these subscriptions, which could be an invitation to bugs	:bug:

**A** You can use the `takeUntil` operator for your subscription that accepts an argument that is a subject that will get destroyed when you `.next()` in `ngOnDestroy`. Only issue with this one is that it has to be the last operator in the `pipe`. This could become a problem if you need to do something such as `shareReplay`. Great article about this issue can be found here: [https://blog.angularindepth.com/rxjs-avoiding-takeuntil-leaks-fb5182d047ef](https://blog.angularindepth.com/rxjs-avoiding-takeuntil-leaks-fb5182d047ef)

**A** Ward Bell came up with [SubSink](https://www.npmjs.com/package/subsink), which is described as a dead simple class to absorb RxJS subscriptions in an array.

---

#### State Management

**Q** If you have a lengthy number of models, is RxJS the answer for handling the state management of these models?

**A** Not really. The more models that you have, the more lines of code that need to be written to handle the state of the model(s).

**Q** So what is a better solution?

**A** You can use ngrx/data, which is an extension of RxJS. As of version 8, it is built into RxJS. 

---

#### How do you scale your front-end angular app?

Have you heard of [**JAMstack**](https://jamstack.org/)? **J** = JavaScript | **A** = API's | **M** = prebuild Markup

---

### Extra

* [Slides](https://www.youtube.com/redirect?q=https%3A%2F%2Fslides.com%2Fjohnpapa%2Fngconf2019-3-angular-tips%23%2F&redir_token=gqCKeAIkanq3aJ94vkhe1QLBXgF8MTU1NzkwNjU4OEAxNTU3ODIwMTg4&v=2ZFgcTOcnUg&event=video_description)
* [ngrx/data](https://www.youtube.com/redirect?q=https%3A%2F%2Fnext.ngrx.io%2Fguide%2Fdata&redir_token=gqCKeAIkanq3aJ94vkhe1QLBXgF8MTU1NzkwNjU4OEAxNTU3ODIwMTg4&v=2ZFgcTOcnUg&event=video_description)
* [VSCode Extensions](https://www.youtube.com/redirect?q=https%3A%2F%2Faka.ms%2Fcode-az&redir_token=gqCKeAIkanq3aJ94vkhe1QLBXgF8MTU1NzkwNjU4OEAxNTU3ODIwMTg4&v=2ZFgcTOcnUg&event=video_description)