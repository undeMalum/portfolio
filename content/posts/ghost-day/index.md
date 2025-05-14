+++
title = "GHOST Day. GHOSTxIRIM revealed!"
date = "2025-05-12T19:12:22+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/me.jpeg"
tags = ["my projects",  "creativity", "service"]
keywords = ["my projects",  "creativity", "service"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
draft = true
+++

# Amplified greetings
It finally happened!

As I mentioned in my [previous posts](/portfolio/posts/engineers-talks/#final-remarks), I attended the [GHOST Day: Applied Machine Learning Conference](https://ghostday.pl). The highlight? Unveiling our [GHOSTxIRIM](https://github.com/GHOST-Science-Club/tree-classification-irim?tab=readme-ov-file#about-the-project) project after six months of hard work.

But I didn’t just present—I networked, met new people, strengthened existing connections, and learned a ton. It was an intense and rewarding two days.

# GHOSTxIRIM
Kuba ([GitHub](https://github.com/rojberr)) and I presented GHOSTxIRIM during the student session—a rapid-fire format of 7-minute talks (+3 minutes Q\&A).

The short time slot pushed us to be razor-sharp. We spent countless hours refining our slides, cutting content, applying feedback, and rehearsing. Honestly, preparing the talk took more time than writing the code!

But it paid off. We received thoughtful questions and encouraging feedback—not just on our talk, but the project itself. It was validating and reinforced our belief in the project's potential impact.

Also, being a speaker with an interesting topic is a great icebreaker—conversations came easily!

More updates are coming once our next steps for GHOSTxIRIM are set. Big things ahead—stay tuned!

# Meaningful encounters
One of the most rewarding parts of the conference was meeting so many new people. Honestly, the last time I connected with this many strangers was probably at summer camp in sixth grade!

I had great conversations, gained fresh perspectives, and learned a lot—from tech topics to tram routes in Poznań.

At the booths, I spoke with [Bartek](https://www.linkedin.com/in/dzionek/) from Google. We had a great tech-focused talk, and he was genuinely interested in our project. He shared a story about an ecology initiative he once contributed to—badgers had become an invasive species in a Polish region, and ecologists were manually sorting thousands of motion-triggered photos. Bartek trained a simple CNN model to detect badgers with \~90% accuracy, drastically improving the workflow. A simple yet impactful use of ML!

I also visited the [G-Research](https://www.gresearch.com) booth, where a funny moment occurred: I greeted the rep in Polish, only to realize he only spoke English. In return, he gave me a generous amount of merch—didn’t want to pack it home, apparently!

The OLX booth was run by a friendly team, but merch came with a twist—you had to play a game to earn it. They weren’t offering internships, but I did later bump into [Jędrzej Kopiszka](https://www.linkedin.com/in/jędrzej-kopiszka-8ab338155/?originalSubdomain=pl) from OLX after his fraud detection lecture. Though I missed the talk, he kindly explained their work—detecting scam listings, trafficking, and explicit content. These problems seem obvious in hindsight, yet I had never considered them in that context. Thought-provoking stuff.

I also had a brief chat with [Chih-Chen Kao](https://drkao.cc) from AMD, who gave a lecture on ray tracing. I didn’t grasp all the technical details, but I approached him afterward to ask about studying at TUM. It turned out he just lives there, but he was incredibly friendly, and we had more time to chat during the afterparty.

Lastly, I met two fellow Student Session presenters:

- [Kacper Cybiński](https://www.linkedin.com/in/kacper-cybiński-5764601b4/) from the University of Warsaw, who talked about using neural networks in physics research—a deep topic that went over my head at first. I caught up with him the next day to dive deeper, and he kindly explained everything.
- [Kacper Wachnik](https://www.linkedin.com/in/kacper-wachnik-5b2860252/) from PUT, who presented his master's thesis project _MisterCar_—a framework for creating AI agents in games, capable of detecting sound or controlling vehicles. He’s even planning to start a new section in the GHOST student organization, which I think is a fantastic initiative.

I could go on, but to sum it up: I met so many fascinating people, and each interaction made the conference even more valuable.

# Non-conventional study experience
When we think of studying or acquiring knowledge, we usually picture a classroom, a lecture hall, or maybe revising at home. But attending a conference, as it turns out, can be an equally powerful and inspiring learning experience.

You may not walk out an expert after a 30- or even 60-minute presentation, but the exposure to new perspectives or entirely unfamiliar topics is incredibly enriching. This was especially true for me during GHOST Day.

One talk that really stuck with me was by [Maya Bechler-Speicher](https://www.linkedin.com/in/maya-bechler-speicher-815103103/) from Meta. Her keynote focused on the limitations of using graphs and graph neural networks (GNNs) in certain domains—particularly biology. She illustrated how graphs may not always capture the full complexity of biological structures. For instance, while proteins can be represented as sequences of amino acids, such a representation can overlook important non-consecutive chemical bonds. That was my main takeaway—along with the idea that making graph embeddings resemble more "regular" graphs often leads to better performance.

After her talk, I approached her to ask more about proteins. While I understood the general benefits of studying protein structures, I wasn’t clear on the specific applications she had in mind. She explained how machine learning can support drug discovery and disease diagnosis by analyzing protein structure. I mentioned my own small project on [HLA-B27 alleles and ankylosing spondylitis](/portfolio/ib-resources/files/ia_bio_final.pdf), as well as a [bachelor’s thesis](/portfolio/posts/engineers-talks/#inflammatory-diseases) that I had an opportunity to get familiar with and that explored inflammatory bowel disease (IBD).

In response, she told me about her team’s recently completed work on an explainable ML model for IBD (if I understood correctly)—which I’m really looking forward to reading.

Continuing on the theme of biology and machine learning, several other talks caught my attention.

One was by Dr. [Krzysztof Krawiec](https://www.linkedin.com/in/krzysztof-krawiec-4119aa38/?originalSubdomain=pl), who presented on neurosymbolic architectures during the computer vision session. His team developed a model that incorporates physical understanding to reconstruct real-world shapes. A particularly interesting application was in thyroid disease detection: the model would generate "healthy" cell structures and identify deviations—i.e., diseased cells—in the image.

Another standout was a talk by [Fatima Sanchez-Cabo](https://www.linkedin.com/in/fatima-sanchez-cabo-b0724b9/?originalSubdomain=es). She showcased her work on cardiovascular risk prediction and the use of chatbots to assist doctors in diagnosis. What impressed me most was the system's high level of explainability: the chatbot would admit when it didn’t know something instead of hallucinating answers—something we still strive for in many AI systems today. It was also a great refresher on cardiovascular biology in general.

There were many more fascinating sessions, but I’ll stop here. I couldn’t attend everything (it was simply impossible), and this post is already getting long. I’ve focused on the talks most relevant to my interests—namely, the intersection of biology and machine learning.

If you're curious about the rest of the event, I highly recommend checking out the [Speakers section](https://ghostday.pl/speakers/) on the GHOST Day website. You might discover someone working on a topic that sparks your curiosity—after all, the internet is a vast resource if you know where to look.

# Closing thoughts
As I mentioned at the beginning, attending the GHOST Day conference was a truly enriching experience.

Learning, networking, eating great food—all packed into just two days!

Earlier that same week, I also attended a smaller event: [KOMPETENTNI.2025](https://www.linkedin.com/events/gdgpozna-xkana-studecki-kompete7324839358769840129/), focused on working in IT. It was a more intimate conference, but I still managed to do plenty of networking—so much so that, like at GHOST Day, I ended up missing a few talks along the way.

In a way, GHOST Day felt like the perfect follow-up. For instance, I had the chance to chat with Bartek about [Google Vertex AI](https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform) and [Model Garden](https://cloud.google.com/vertex-ai?hl=en)—topics I had just learned about a few days earlier from [Ewelina](https://www.linkedin.com/in/ewelinaskowron/) and [Weronika](https://www.linkedin.com/in/weronika-witek/) during KOMPETENTNI.2025.

And that’s just one example. I could easily list more. The point is: conferences are awesome! Tiring, yes—but absolutely worth it. I have no regrets about the time I spent at either event.

Before I wrap up, I want to extend my heartfelt thanks to the GHOST Day organizers. Pulling off such a high-quality conference entirely pro bono—as students—is nothing short of extraordinary. It was an inspiring effort and a pleasure to be part of.

I also want to thank my GHOSTxIRIM teammates for their amazing contributions to the project. Their advice, technical skills, and dedication—especially while many were also involved in organizing the event itself—were truly impressive.

Now, plans are underway to integrate GHOSTxIRIM with QGIS, and I’m excited for what comes next. There's also a party coming up for all AI majors from our study year, and the semi-finals of the academic Polish football championships are just around the corner. That event definitely deserves its own post—especially since I haven’t written about sports in a while!

That’s all for now. Thanks for reading—and see you soon!
