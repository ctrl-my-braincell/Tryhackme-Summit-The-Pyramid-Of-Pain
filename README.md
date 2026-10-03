# Applying the Pyramid of Pain in Summit hands on lab

## Objective

In the TryHackMe **Summit** room, I participated in a simulated purple-team exercise focused on malware detection and detection engineering.

The scenario involved PicoSecure working with an external penetration tester who attempted to execute different malware samples on an internal workstation. My role was to act as the defender and configure security controls capable of detecting and preventing each sample.

The exercise follows the **Pyramid of Pain**, where detections gradually move from simple Indicators of Compromise (IOCs), such as file hashes, toward behavioral indicators and attacker Techniques, Tactics, and Procedures (TTPs).

The main idea is that the higher a detection moves up the Pyramid of Pain, the more difficult and costly it becomes for an attacker to adapt.

## The Pyramid of Pain

Before completing the practical, I learned that indicators can be organized based on how difficult they are for an attacker to change.

From lowest to highest:

1. **Hash Values**
2. **IP Addresses**
3. **Domain Names**
4. **Network/Host Artifacts**
5. **Tools**
6. **Tactics, Techniques, and Procedures (TTPs)**

For example, detecting a malicious executable using its hash is useful, but the attacker can modify the file and produce a completely different hash.

If the detection instead focuses on the attacker's behavior, the attacker may need to change the way their attack works rather than simply modifying a file or changing infrastructure.

This is where the "pain" in the Pyramid of Pain comes from.

## Purple-Team Scenario

The Summit room demonstrates this concept through an iterative purple-team exercise.

The simulated attacker attempts to execute a malware sample, while the defender analyzes the available indicators and creates a detection or prevention rule.

Once the defender successfully detects the malware, the attacker changes something about their attack and attempts to bypass the previous detection.

This creates a cycle:

**Attack → Detect → Block → Attacker Adapts → Improve Detection**

Instead of relying on a single IOC, the defender is gradually forced to develop stronger detections.

## Practical Investigation

During the exercise, I analyzed multiple malware samples:

- `sample1.exe`
- `sample2.exe`
- `sample3.exe`
- `sample4.exe`
- `sample5.exe`
- `sample6.exe`

Each stage demonstrated a different way of detecting malicious activity and showed how an attacker could adapt when a particular indicator was blocked.

Rather than treating every malware sample as an unrelated challenge, I viewed them as iterations of the same attacker-defender interaction.

The important question became:

> **What am I detecting, and how difficult would it be for the attacker to change it?**

This helped connect the practical exercise back to the Pyramid of Pain.


## Key Takeaway

My biggest takeaway from this room was that **not all detections have the same long-term value**.

Hashes, IP addresses, and domains are still useful indicators, especially during incident response. However, they can often be changed relatively easily by an attacker.

Moving toward host/network artifacts, tools, and ultimately TTP-based detections makes evasion increasingly expensive because the attacker must change larger parts of their operation.

The Summit exercise helped me understand detection engineering as an iterative process rather than simply identifying whether something is malicious.

A good detection does not only ask:

**"Can I detect this malware?"**

It also asks:

**"What would the attacker have to change to bypass my detection?"**

And this way, this room not only taught me the importance of defensive/blue teaming, but also thinking like an attacker and put that into my defense, as to detect them more sharply - that is a good practice to purple teaming! <3
