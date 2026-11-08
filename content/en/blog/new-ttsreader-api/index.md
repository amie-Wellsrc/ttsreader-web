---
title: "From Product to Platform: The Story of the TTSReader API"
description: "An inside look at why we built the TTSReader API. Go beyond the web player and learn how to integrate our powerful TTS engine into your own applications."
excerpt: "We wanted to see what you could build. This is the story of how we opened up our TTS engine through a RESTful API, focusing on an asynchronous, webhook-driven workflow for developers."
date: 2026-01-08T10:00:00+02:00
lastmod: 2026-01-11T10:25:00+02:00
draft: false
weight: 50
images: []
categories: ["API", "Story"]
tags: ["api", "development", "automation", "webhooks", "platform"]
contributors: ["Amiel"]
pinned: false
homepage: false
---

*By Amiel | Jan 11, 2026*

For a long time, [TTSReader](https://ttsreader.com/player/) was a destination—a place you would go to convert text to speech. But we started to wonder: what if it could be a service? What if, instead of you coming to us, we could bring the power of our TTS engine to you, inside your own applications?

This question marked our transition from a *product* to a *platform*. The result of that transition is the **TTSReader API**, a set of tools for developers to build their own audio-powered applications.

### The Vision: An Engine for Your Ideas

We saw developers using our player in creative but manual ways: pasting text, downloading the MP3, and then using that file in their own project. It worked, but it wasn't scalable. We wanted to remove the friction and give developers direct, programmatic access to our synthesis engine.

Our design philosophy was to create a RESTful API that was both powerful and easy to integrate, focusing on a modern, asynchronous workflow that wouldn't tie up developers' own systems.

### How It Works: A synchronous (for now), Webhook-First Design


1.  **Authentication & Job Submission:** You start by authenticating with your secret key and submitting a job to one of our endpoints (e.g., `ttsSync` for short tasks). For longer jobs, this request immediately returns a `taskId`.
2.  **The Webhook System:** This is the core of our asynchronous design. Instead of you repeatedly asking "Are you done yet?" (polling), you can provide a webhook URL when you submit a job.
3.  **The Notification:** Once the audio synthesis is complete, our server sends a `POST` request to your webhook URL. This request contains a JSON payload with the `taskId`, status, and, most importantly, the public URL of the finished MP3 file.

This webhook-first approach means your application can fire off a request and then forget about it. When the audio is ready, our server lets you know. It's a more efficient and scalable way to build integrations.

### What Will You Build? Some Ideas to Get You Started

The API is a blank canvas. Here are a few stories of what's possible:

*   **The Automated Blog:** A developer creates a plugin for their Ghost blog. Using the CMS's own webhooks, a new post triggers a call to the TTSReader API. When our webhook returns the finished MP3 URL, the plugin automatically edits the post to include an embedded audio player. The "audio version" of the blog is now created and published in seconds, with zero manual effort.
*   **The Accessible News App:** A mobile developer builds a news aggregator app. When a user opens an article, they see a "Listen" button. Tapping it sends the article text to our API. The app then streams the audio directly from the URL we provide, turning any article into a personal podcast episode.
*   **The Smart Home Narrator:** A home automation enthusiast uses Node-RED to connect their smart home events to our API. Instead of a generic chime, their system now gives helpful voice notifications like, "Reminder: The laundry will be finished in five minutes," or, "Warning: A water leak has been detected in the basement."

The TTSReader API is our invitation to the development community. We've built a powerful engine; now we want to see what you'll build with it.

<div style="padding:20px;border-radius:8px;border:1px solid var(--border-color)"><h3 style="margin-bottom:20px">API Examples</h3><div style="margin-bottom:25px"><div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px"><h4 style="margin:0">cURL Example</h4></div><pre style="background:#1e1e1e;color:#d4d4d4;padding:15px;border-radius:6px;overflow-x:auto;font-size:13px;line-height:1.4"><button class="btn btn-copy"></button>curl -X POST "https://ttsreader.com/api/ttsSync" \
-H "Authorization: Bearer UAPI-<span style="color:#4fc3f7">YOUR_SECRET_KEY</span>" \
-H "Content-Type: application/json" \
--data '{"text": "Hello world, this is a test from TTSReader API.","lang": "en-US", "voice": "Nova Premium", "rate":1, "quality": "48khz_192kbps" }' \
--output "hello.mp3"</pre></div><div><div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px"><h4 style="margin:0">JavaScript Example</h4></div><pre style="background:#1e1e1e;color:#d4d4d4;padding:15px;border-radius:6px;overflow-x:auto;font-size:13px;line-height:1.4"><button class="btn btn-copy"></button>    <span style="color:#569cd6">const</span> <span style="color:#9cdcfe">response</span> = <span style="color:#569cd6">await</span> <span style="color:#dcdcaa">fetch</span>(<span style="color:#ce9178">'https://ttsreader.com/api/ttsSync'</span>, {
        <span style="color:#9cdcfe">method:</span> <span style="color:#ce9178">'POST'</span>,
        <span style="color:#9cdcfe">headers:</span> {
            <span style="color:#ce9178">'Authorization'</span>: <span style="color:#ce9178">'Bearer UAPI-<span style="color:#4fc3f7">YOUR_SECRET_KEY</span>'</span>,
            <span style="color:#ce9178">'Content-Type'</span>: <span style="color:#ce9178">'application/json'</span>
        },
        <span style="color:#9cdcfe">body:</span> <span style="color:#dcdcaa">JSON.stringify</span>({
            <span style="color:#9cdcfe">text:</span> <span style="color:#ce9178">"Hello world, this is a test from TTSReader API."</span>,
            <span style="color:#9cdcfe">lang:</span> <span style="color:#ce9178">"en-US"</span>,
            <span style="color:#9cdcfe">voice:</span> <span style="color:#ce9178">"Nova Premium"</span>,
            <span style="color:#9cdcfe">quality:</span> <span style="color:#ce9178">"48khz_192kbps"</span>,
            <span style="color:#9cdcfe">rate:</span> <span style="color:#b5cea8">1</span>
        })
    });
</pre></div><br><h3 style="margin-bottom:15px">API Parameters</h3><ul style="line-height:1.8;margin-left:20px"><li><b>text</b> (required): The text to convert to speech</li><li><b>lang</b> (required): Language code (e.g., "en-US", "es-ES")</li><li><b>voice</b> (required): The name of the voice is as shown in the voice-selector UI. In addition ALL Azure's Neural AI voices are supported. So, you're welcome to go to Azure's voice gallery at <a href="https://speech.azure.cn/portal/voicegallery" target="_blank" style="color:#007bff">https://speech.azure.cn/portal/voicegallery</a> and select a voice from there.</li><li><b>rate</b> (optional): Speech rate (0.5 to 2.0, default: 1.0)</li><li><b>quality</b> (optional): "48khz_192kbps" or empty (which defaults to 24kHz)</li></ul><br></div>

For a full list of endpoints and parameters, dive into the complete **[API Documentation](https://ttsreader.com/api/)**.
