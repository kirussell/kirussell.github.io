---
layout: post
title:  "Get to know AOSP. I had a struggle to navigate to a Service."
date:   2023-06-15 14:00:00 -0500
categories: blog post
---
This is one of those posts where I could have spilled out the answer in one sentence, but I decided to put a whole story behind this small but probably obvious finding of mine about AOSP. When you are an Android Apps developer and are onboarded to a team that builds OS for VR headsets thats a bit of a challenge. Everything is so familiar but so different.

![wokring_with_vr_headset](/assets/images/facebook_vr_headset_working.png)

That was 2019 half a year before the pandemic started, I joined Oculus, funny enough plenty of people did not even know it was part of Facebook. I joined the VROS team that was building the best operating system for VR, then MR, and hope AR in the future. I was excited to join VR space with my experience in Android App development: migrations to Dagger, constantly checking better RxJava operators suitable to display a list of items, applying MVP then MVVM then Unidirectional flow, moving Views on screen left 3 pixels then right 4. It was a new area for me and an unknown territory of working with AOSP (VROS is Android under the hood).

While being a (I hope) decent Android App developer I never understood why would I need Android Services at all. The only thing I remembered about working with services was a super old presentation about HTTP requests in the background where an example showed a service that was using HttpUrlConnection to joggle bytes back and forth. I was always unsure why would it need a Service. One of a few places I dealt with Services as an App developer were all these fancy SDKs and libs that used Services to “grind” things in the background, but you just call SDK API and don't care much about Services implementations (unless there is a bug).

So I had a warm welcome from VROS or, actually, AOSP which is mainly Android Services or SystemServices talking to each other through IPCs, HALs, Broadcasts, and Intents. Now it's not Activity lifecycle quirks anymore but weekdays of going through Java abstractions and JNI to get to native code and lots of times an AIDL to get to underlying Service behaviors. And if in the case of Broadcasts and Intents as an App developer, you know how to search actions and find raw string representations (i.e. [NEXT_ALARM_CLOCK_CHANGED](https://cs.android.com/search?q=NEXT_ALARM_CLOCK_CHANGED&sq=&ss=android%2Fplatform%2Fsuperproject)), and JNI is also in most cases searchable by method names cause those are carefully named and can be distinguished from everything else, but for AIDLs, I was scratching my head for some time.

AIDL, if you don’t remember, is ****[Android Interface Definition Language](https://developer.android.com/guide/components/aidl)** - an interface that can be implemented somewhere in the code base and cannot be “physically” (as much as this word is applicable to code) connected to implementation if you would be able to use some smart IDE and of course, there is no IDE with indexed search (like ctrl+B in IntelliJ IDEA) that can decently work with whole AOSP. So text search it is - https://cs.android.com/android/platform/superproject.

I.e. [TelephonyManager](https://cs.android.com/android/platform/superproject/+/master:frameworks/base/telephony/java/android/telephony/TelephonyManager.java?q=TelephonyManager&ss=android%2Fplatform%2Fsuperproject) class which is part of the Android framework that provides “information about telephony services on the device”. It uses [ITelephony.aidl](https://cs.android.com/android/platform/superproject/+/master:frameworks/base/telephony/java/com/android/internal/telephony/ITelephony.aidl?q=ITelephony.aidl&ss=android%2Fplatform%2Fsuperproject) under the hood to communicate which other system services that talk to deeper layers to get info. So if you need to find an implementation of the class behind that interface the most obvious thing is to [search the exact word “ITelephony”](https://cs.android.com/search?q=ITelephony&sq=&ss=android%2Fplatform%2Fsuperproject) which gives you several hundreds of places to check. Not a super efficient spend of time to go through all of them (which I spent).

![code_search_itelephony](/assets/images/code_search_itelephony.png)

I looked at other examples to make it easier for me to search and will show here just one to prove the point (in reality I spent a few weeks with different tasks to realize how stupid I was). PowerManager which does not need an introduction and it uses IPowerManager.aidl which I would expect to be implemented by PowerManager, but no it's not since it uses one. Same steps - searching “IPowerManager” and luckily high enough in search results:

![code_search_ipowermanager](/assets/images/code_search_ipowermanager.png)

So “IPowerManager.Stub” or even better “extends IPowerManager.Stub” seems like a reasonable query to use. And yes it works with the original case - [“extends ITelephony.Stub”](https://cs.android.com/android/platform/superproject/+/master:packages/services/Telephony/src/com/android/phone/PhoneInterfaceManager.java?q=%22extends%20ITelephony.Stub%22&ss=android%2Fplatform%2Fsuperproject) gives exactly one result to be 100% sure it is the implementation I need to mess around with.

Pretty simple. At some point, I even tried to ask ChatGPT, Bard, and Bing to point me to the implementation of a specified aidl, so AI is not there yet (close but not correct), Im not losing my job at least this month.

![aidl_impl_chat_gpt](/assets/images/aidl_impl_chat_gpt.png)

If you want to find code that receives and handles calls through AIDL search for “*AIDLInterfaceName.Stub”* or share a better approach in comments (I would really appreciate it).

It’s always great to learn something new and even after a decade of working with Android as an App developer diving into AOSP is exciting!

Are you an App developer or AOSP?
