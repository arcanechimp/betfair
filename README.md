Betfair Golang Library
===
Betfair is the leading company in online betting exchange. They are currently developing the next generation API for developers which want to automate their betting systems. For more info, please refer to [Betfair API-NG home](https://api.developer.betfair.com/services/webapps/docs/display/1smk3cen4v3lu3yomq5qye0ni/API-NG+Overview).

This is a library for Golang. Please note that this is a pre-alpha version, so at the moment only a subset of the API methods are implemented. However, the project is active so new commits will be done frequently.

I will write some articles on the library [here](aded.it/tag/comp/betfair-golang-library).

TODO
---
The only login method implemented is the [non-interactive (bot) login](https://api.developer.betfair.com/services/webapps/docs/display/1smk3cen4v3lu3yomq5qye0ni/Non-Interactive+%28bot%29+login).

Account API methods have to be implemented, as well as some Betting API methods.

Methods implemented:

* LoginNonInteractive
* KeepAlive
* Logout
* ListCompetitions
* ListCountries
* ListEvents
* ListEventTypes
* ListMarketBook
* ListMarketCatalogue

Quick usage
---
I will provide more examples and documentation ASAP.
<pre>
package main

import (
        "github.com/aded/betfair"
        "fmt"
        "log"
)

func main() {
        // Configuration
        config := new(betfair.Config)
        config.Exchange = "UK" // UK or AU
        config.CertFile = "YOUR_CERT_FILE_HERE"
        config.KeyFile = "YOUR_KEY_FILE_HERE"
        config.Username = "YOUR_USERNAME_HERE"
        config.Password = "YOUR_PASSWORD_HERE"
        config.AppKey = "YOUR_APPKEY_HERE"
        config.Locale = "en"

        // Create a new session
        s, err := betfair.NewSession(config)
        if err != nil {
                log.Fatal(err)
        }

        // Create a market filter passed as arg to ListCompetitions
        marketFilter := new(betfair.MarketFilter)
        marketFilter.EventTypeIds = []string{"1"}
        results, err := s.ListCompetitions(marketFilter)
        if err != nil {
                log.Fatal(err)
        }
        for _, result := range results {
                fmt.Printf("%s (%d) from %s\n",
                        result.Competition.Name, result.MarketCount, 
                        result.CompetitionRegion)
        }

        // Do other stuff...

        // KeepAlive
        if err = s.KeepAlive(); err != nil {
                log.Fatal(err)
        }
 
        // Logout
        if err = s.Logout(); err != nil {
                log.Fatal(err)
        }
}
</pre>

Tests
---
To run tests, you have to provide your credentials in the file
<pre>
betfair_test.conf.json
</pre>
You can find a sample configuration file in the main directory:
<pre>
betfair_test.conf.json.sample
</pre>

License
---
The library is dual licensed. For free software/open source projects, please refer to GPLv3. For commercial projects, please contact the author.

Note
---
I apologize for my poor English :-).