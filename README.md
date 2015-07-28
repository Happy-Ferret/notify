### `notify`

Notify is a go library for interacting with the dbus notification service defined here:
https://developer.gnome.org/notification-spec/

It can deliver notifications to desktop using dbus communication, ala how libnotify does it.
It has so far only been testing with gnome and gnome-shell 3.16 in Arch Linux. 

More tester are very welcome =)

Depends on github.com/godbus/dbus.

#### Quick intro

```go
package main

import (
	"github.com/esiqveland/notify"
	"github.com/godbus/dbus"
	"log"
	"fmt"
)

func main() {
	iconName := "mail-unread"

	conn, err := dbus.SessionBus()
	if err != nil {
		panic(err)
	}

	notifier := notify.NewNotifier(conn)

	n := notify.Notification{
		AppName:       "Test GO App",
		ReplacesID:    uint32(0),
		AppIcon:       iconName,
		Summary:       "Test",
		Body:          "This is a test of the DBus bindings for go.",
		Actions:       []string{},
		Hints:         map[string]dbus.Variant{},
		ExpireTimeout: int32(5000),
	}

	id, err := notifier.SendNotification(n)
	if err != nil {
		panic(err)
	}
	log.Printf("sent notification id: %v", id)
```

You should now have gotten this notification delivered to your desktop.
