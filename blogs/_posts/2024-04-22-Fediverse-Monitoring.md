---
layout: post
title: "Fediverse Monitoring"
sitemap: true
hide_last_modified: true
---

The Fediverse represents a [decentralized network](https://gist.github.com/joepie91/f924e846c24ec7ed82d6d554a7e7c9a8), a cluster of independent servers each hosting a diverse social galaxy ranging from art and literature to technology, where users can engage across different communities without the constraints of traditional social media platforms. Far from the clout-chasing arenas frequented by celebrities and algorithms, this network cherishes building close-knit, human-centered communities while [shunning the reliance on ads, trackers, or the flashy allure of cryptocurrency](https://gist.github.com/joepie91/f924e846c24ec7ed82d6d554a7e7c9a8).

The Fediverse operates on a [decentralized network structure](https://www.theverge.com/24063290/fediverse-explained-activitypub-social-media-open-protocol), primarily utilizing the [ActivityPub protocol](https://mashable.com/article/what-is-the-fediverse), which is overseen by the World Wide Web Consortium. This protocol allows various platforms within the Fediverse, such as Mastodon, PeerTube, and Pixelfed, to [interoperate seamlessly](https://opensource.com/article/23/3/tour-the-fediverse). Users can interact across these different platforms without needing separate accounts, thanks to the federated nature of the network. This setup not only enhances user convenience but also upholds the principles of data privacy and open-source software, with the majority of Fediverse platforms being [free to use and modify](https://en.wikipedia.org/wiki/Fediverse).

By allowing users to host their servers and manage their data, the Fediverse [reduces the risk of data breaches or misuse](https://techwireasia.com/07/2023/heres-why-the-fediverse-is-empowering-users-like-never-before/). This network presents an opportunity to reconstruct the internet infrastructure in significantly improved ways, where users can [access their content across multiple platforms without needing multiple usernames or profiles](https://www.theverge.com/23990974/social-media-2023-fediverse-mastodon-threads-activitypub), thus [simplifying online interactions](https://appliednetsci.springeropen.com/articles/10.1007/s41109-021-00392-5).

![fediverse](/assets/img/fed/fed.png)


### Monitoring System

In my internship at Cyfinoid Research, I built a fediverse monitoring system. Nearly all fediverse platforms are free and open-source software. Due to the open nature of protocols a lot of details are available if you check the right API.

This project gathers up the following statistics of fediverse:

- Top 10 server software
- Top 10 Server Software + Version
- Top 10 TLD (Top level domain)
- Daily server count
- Daily users count
- Daily posts count
- Daily comments count

I used GoLang to fetch data through API and store in PostgresSQL. Chart.js is used to visualize data.

Moreover, I am running cron job to fetch the updated data alternate day.

### Screenshots of the dashboard

![fediverse](/assets/img/fed/1.png)
![fediverse](/assets/img/fed/2.png)


### Contribute to the project:

This project recently opened its doors to the open source community. If you've ever been curious about contributing to open source but felt a bit intimidated, this project is a perfect entry point. Their step-by-step approach makes it easy for anyone to jump in and make a difference.


💡 Github Link: [Go ahead and contribute!](https://github.com/cyfinoid/fediverse-monitoring) 
