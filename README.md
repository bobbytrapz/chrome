Package for remote controlling with Chrome DevTools. Requires Google Chrome.

Very much a work in progress. Features are only added as I need for my other projects.

The api is not final and not even particularly well thought out. I just needed something quick and dirty. I'm still figuring a lot out. But this is a cool project and I will likely return to clean it up.

There is no support for Windows but I hope there will be eventually.

## Take a screenshot

```go
package main

import (
	"context"
	"github.com/bobbytrapz/chrome"
)

func main() {
	link := "https://google.com"
	userProfileDir := "example-profile"
	port := 9991
	ctx := context.Background()
	ctx, cancel := context.WithCancel(ctx)

	// start chrome
	if err := chrome.Start(ctx, userProfileDir, port); err != nil {
		panic(err)
	}

	// wait for chrome to close
	defer func() {
		chrome.Wait()
	}()

	tab, _ := chrome.ConnectToNewTab(ctx)
	tab.PageNavigate(link)
	tab.WaitForLoad()
	tab.PageCaptureScreenshot("google.png")
	cancel()
}

```
