---
date: 2018-07-23
subtitle: "The Soul Exchange - Vow of Seth"
title: Lighting Breakdown for a Music Video
---

## The Video
Before we get into the details, I figure you might be interested in seing the results.
Take a look and continue reading below when you're finished!

{{< vimeo 253673229 >}}

## Introduction
I've always appreciated reading about how things are actually done behind the scenes, so 
it seems just right that I would share my own experiences and thoughts when going about a project.

Last autumn I was asked to shoot a music video for the band The Soul Exchange. We didn't have much of a budget
but we had a cool location and a great but small crew. Directed by Ludvig Gür and produced by Najka Pictures I knew 
that it was going to be a great time. 

We shot the music video over two days. Day 1 was the story part of the video and day 2 was shooting all the band members.
All of it took part in [Bogesund Castle](https://goo.gl/maps/EKSzBHcfHZw). We used pretty much every room in there in order 
to get some variation in the shots.

## The Gear
As we had a low budget I and great gaffer [Kamil Janowski](http://www.kjanowski.com/) figured using 
Tungsten as the main lighting would be a good idea. We planned to use the hard light from par cans to 
be our main source and to haze up the rooms for added atmosphere and depth. Unfortunately we were 
forbidden by the landlord from any hazing at all. We instead opted for Tiffen Smoque, a filter that is
supposed to mimic the effect of haze without actually using any. 

{{< figure src="/img/vos/Smoque.jpg" title="[Smoque-filter](https://tiffen.com/diffusion/) in action" >}}

Obviously we didn't get the same 
depth that real haze gives, but I was surprised by how well it worked giving a sense of haziness in the room. It benefitted us greatly that we had so many light sources pointing straight into the lens making the effect much more pronounced.

We also added a dedolight kit for very precise lighting (I'll get to that), a dedolight kit, 2 LED-panels 
bi-colour and some very versatile 3-foot Pipeline Free from [BB&S] (https://bbsrentalsupport.com/collections/pipeline-system/products/pipeline-free-5600k-1-feet)

Since we were going for a dark look but we were shooting during the day, we had to get some black cloth and flags to block out the sunlight from the windows as well.

We wanted to have some camera movement but with our limited time I figured a wheelchair and shoulder mount would be best. This way we could get some movement that was steadier than walking but still move
fast.

#### So what we ended up using was: <br>

##### Lighting
* 5 Par cans 1kW
* 1 Dedolight kit 3x150W
* 2 1x1 LED-panels Bi-colour
* 4 3-Foot Pipeline Free
* Flags and black cloth

##### Camera
* Red Epic Dragon 6K 
* Sigma Art-lenses
* [Tiffen Smoque 2](https://tiffen.com/diffusion/)
* Tiffen ND-filters
* Bright Tangerine Misfit Atom
* DJI Follow Focus
* Wheelchair


## Scene 1 - Drummer

{{< figure src="/img/vos/Drums1.jpg" title="Note the par cans behind" >}}

What's great about this video is that we knew from the start that we wanted lights inside the shot
so we never had to worry about limitations for the lights. I wanted to try to be creative with the rooms we had and to get some variation between the shots. This was the first shot we did with any instrument.

{{< figure src="/img/vos/Drums2.jpg" >}}

We placed three par cans behind the drummer pointed towards him, you can see them in the shot. Then
we added a single pipeline just outside of frame to bring up the exposure of the drummer. Great output
in such a small light. The final touch was to use a dedo pointed at the bass drum to bring up the 
exposure of the band logo.

{{< figure src="/img/vos/VoS1.jpg" >}}


## Register the Scaleway node to Docker Cloud
Once registered, we need to add our Scaleway node as a Docker node.
First things first, Docker Cloud requires us to open a
couple of inbound ports, namely `2375/tcp`, `6783/tcp` and `6783/udp`.
While we're here, you probably want to add an inbound rule
for `443/tcp` (you do plan to serve your app over HTTPS, right?).

Once the security group has been configured, go back to Docker
Cloud and lick the `+` button in the top right and select
`Bring Your Own Node`. It'll give you a command that we need
to run on our Scaleway node, so now it's time to ssh onto
the Scaleway node.

```
ssh root@your-scaleway-node-ip
```

Once you've accepted the host key, proceed to run the snippet
from the Docker Cloud page. If everything works, your Docker Cloud
page should tell you that the node was successfully registered!

## Sidenote: Multi-stage Docker Builds
At this point I want to mention that this works particularly
well if you utilize multi-stage docker builds. They're a new
feature in Docker `17.05`, so ensure you've selected `17.05`
as the docker version in your Docker Cloud settings.
See this `Dockerfile` for an example:

```Docker
# Build
FROM golang AS build
ADD . /go/src/github.com/myrepo/myapp
ENV CGO_ENABLED=0
RUN cd /go/src/github.com/myrepo/myapp && go build -o /app

# Production
FROM scratch
COPY --from=build /app /app
EXPOSE 443
ENTRYPOINT ["/app"]
```

We use the [official golang image](https://hub.docker.com/_/golang/)
to build the application, then we just take the static
binary and stuff it into a minimal container environment.
Amazingly simple and you end up with a container not much
larger than the size of the binary itself. Truly we are
living in the future.

## Setting up the Docker Cloud repository
We've added Docker Cloud to our GitHub already, so now
we can go ahead and create a repository from GitHub. I'm assuming
you've already got your source repo on github so it should
just be a matter of clicking `Create` and selecting
the repository to link it to in the settings. It'll
automatically detect if there is a `Dockerfile` in the root
of your repository. Otherwise, just select the path to the
`Dockerfile`. Your new Docker Cloud repository will automatically be configured to build on new merges to master.

## Deploying the app
Go to the repository page we just created. See that alluring
`Launch Service` button in the top right? It's time to launch
our service! Click it and on the next page you'll get a new interface
allowing you to customize the forwarded ports, volumes, commands
and many other things. Most significantly, make sure to turn on
`AUTOREDEPLOY`. This will automatically redeploy your service
when a new one has been successfully built from your source.

Once you've tweaked the dials and dotted the i's, go ahead and
click `Create & Deploy`. It'll spin up your new container
on the Scaleway node we registered earlier. Magic!

## 🍾

Congratulations! You've now got your demo app running in a
Docker container on your Scaleway node, with automatic
redeployment triggered straight from your GitHub pushes.
Lean back in your chair and crack open a cold one, you deserve it!

If you liked this article, or you have anything you'd like to add
or correct, don't hesitate to reach out to me on twitter
[@johanbrandhorst](https://twitter.com/JohanBrandhorst) or on
Gophers Slack under `jbrandhorst`. Thanks for reading!
