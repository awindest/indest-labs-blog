---
title: The Data Governance Story
description: An interesting explanation of modern governance.
slug: data-governancing
published: '11-15-2026'
category: governance
---
<script lang="ts"> 
import Embed from '#lib/components/embed.svelte' 
import Image from '#lib/components/image.svelte' 
import YouTube from '#lib/components/youtube.svelte' 
// import Wheel from './dmbok-wheel.svelte' 
</script>

## DAMA-DMBOK Data Management Wheel
	<!-- <p class="intro"> -->
    
### Hover over, tap, or keyboard-focus a segment to see what it covers.

<!-- <Wheel></Wheel> -->

## Table of Contents

### Introduction

### Disclaimer

### Shifting Focus

### Not Just a Technology Problem

### Why?

## Why do we need Data Governance?

### What?

### You Need to Understand Your Data

### Who?

## Three Key Persona Categories in Data Governance

### Business:

Report users
Subject experts
Data experts
Line of business
Data stewards
Policy experts
Executives

### Architect

Systems architects
Database architects
Business Intelligence (BI) architects
Data Architect

### Developer:

Database administrators
Business Intelligence (BI) developers
Application developers

## Governance Responsibilities

### How?

## Data Governance Deliverables - A Sample

1. Business Problem
2. Business Case
3. Critical Data Elements
4. Business Glossary
5. Business Rules
6. Allowable Values
7. Metadata & Data Lineage
8. Data Profiling/Discovery
9. Data Validation against Business Rules
10. Data Quality Dashboards

# AI

AI brings more complexity onto the governance landscape as the issues are amplified exponentially. 

## AI Risks

These seven categories match the high-level risk domains in the MIT AI Risk Repository. Each section below defines the category, gives example failures, and notes how governance typically responds.

1. Discrimination & Toxicity
1. Privacy & Security
1. Misinformation
1. Malicioius Actors
1. Human-Computer Interaction
1. Socioeconomic & Environmental
1. AI System Safety, Failures & Limitations

## The Seven Categories

1. Discrimination & Toxicity

This category covers AI systems that produce unfair outcomes for particular groups, or that generate harmful, hateful, or abusive content. It includes unfair discrimination and misrepresentation, exposure to toxic content, and unequal performance across groups. Governance treats it as a rights and fairness issue, which is why it appears in the EU AI Act’s high-risk provisions and in anti-discrimination law.

Bias usually enters through training data that reflects historical inequity, through proxy variables, or through under-representation of certain groups. Reuters reported in 2018 that Amazon scrapped an experimental recruiting tool that penalized résumés containing the word “women’s.” The COMPAS recidivism tool was criticized for different error rates across racial groups. Facial recognition systems have shown higher error rates for darker-skinned women, and some have led to wrongful arrests.

Toxicity failures happen on the generative side. Microsoft’s Tay chatbot was manipulated into producing racist content within hours of launch in 2016. Modern LLMs can still produce slurs, stereotypes, or harassment, especially under adversarial prompting.

Governance responses include bias audits, representative test sets, disaggregated performance metrics, impact assessments before deployment, content filters, and human review for high-stakes decisions. Fairness definitions can conflict mathematically, so organizations need to document which one they chose and why.

2. Privacy & Security

This category covers the risk that AI systems expose personal or confidential information, or that the systems themselves are compromised. It has two halves. The privacy half concerns leakage of sensitive data and the ability to infer private attributes. The security half concerns vulnerabilities in AI systems and attacks on them.

Privacy failures include models memorizing and regurgitating training data, and employees pasting confidential material into public tools. In 2023, Samsung engineers reportedly pasted proprietary source code into ChatGPT, which led the company to restrict its use. Models can also infer sensitive traits such as health conditions or sexual orientation from innocuous data, and large-scale scraping raises consent and data-protection problems under GDPR and similar laws.

Security failures are specific to AI: prompt injection, where hidden instructions in a web page or email hijack an AI agent; data poisoning of training sets; model extraction and theft; and adversarial inputs, such as stickers on a stop sign that cause misclassification. Agentic systems with tool access raise the stakes, because an injected instruction can trigger real actions like sending data or making purchases.

Governance responses include data minimization, privacy impact assessments, access controls, red-teaming, vendor due diligence, acceptable-use policies for employees, and security frameworks such as NIST AI RMF and MITRE ATLAS.

3. Misinformation

This category covers AI generating or amplifying false or misleading content, whether or not anyone intends harm. It includes hallucinations (confident but fabricated outputs), erosion of shared understanding when synthetic content floods information ecosystems, and personalization that deepens polarization. The key distinction from “Malicious Actors” is intent: here the falsehood is often an unintended product of the system.

The best-known example is legal. In Mata v. Avianca (2023), lawyers submitted a brief citing court cases that ChatGPT had invented, and they were sanctioned. In 2024, a tribunal held Air Canada liable after its chatbot gave a customer incorrect bereavement-fare policy information, rejecting the argument that the chatbot was a separate entity. Hallucinated medical, financial, or technical advice can cause direct harm when users trust it.

At a societal level, AI-generated articles, reviews, and images can crowd out reliable sources, and recommendation systems can push users toward increasingly extreme or false content. Over time, people may also stop trusting authentic content, which is sometimes called the “liar’s dividend.”

Governance responses include grounding outputs in verified sources, citation requirements, uncertainty signaling, content provenance standards such as C2PA, human review in high-stakes domains, and clear allocation of liability to deployers.

4. Malicious Actors

This category covers deliberate misuse of AI by people seeking to harm others. AI lowers the cost, skill, and time needed for fraud, manipulation, cyberattacks, and (in the most serious scenarios) weapons development. Governance here is closer to security and law enforcement than to quality assurance, because the adversary is intelligent and adapts.

Fraud and impersonation are the most visible cases. In 2024, an employee at the engineering firm Arup was reported to have transferred about $25 million after a video call with deepfaked “executives.” Voice-cloning scams target families with fake emergencies. Non-consensual intimate deepfakes have caused serious harm to victims, including minors. AI also enables personalized phishing at scale and automated influence operations that run networks of fake personas.

Cyber misuse includes using models to write or refine malware, find vulnerabilities, or automate reconnaissance. More severe concerns involve uplift toward biological, chemical, or other mass-casualty capabilities, which is why frontier labs run dangerous-capability evaluations before release.

Governance responses include usage policies and enforcement, abuse monitoring, safety training and refusals, capability evaluations, know-your-customer rules for powerful models, watermarking and provenance, and criminal liability for specific misuses. Open-weight models complicate this, since safeguards can be removed after release.

5. Human-Computer Interaction

This category covers harms that arise from the way people interact with AI systems, even when the system works as designed. The main concerns are overreliance and unsafe use, loss of human agency and autonomy, and anthropomorphism that leads people to trust or bond with systems inappropriately.

Overreliance is common where AI sits inside professional workflows. Clinicians may defer to a flawed diagnostic suggestion, or drivers may disengage while using partially automated driving features, as in several Tesla Autopilot crashes. This is often called automation bias. When humans are formally “in the loop” but rubber-stamp outputs, oversight becomes a legal fiction rather than a real safeguard.

Emotional dependence is another failure mode. Companion chatbots can encourage intense attachment, and there have been lawsuits and public concern about teen users of companion apps, including cases where vulnerable users received harmful responses. When Replika changed its model’s behavior in 2023, many users reported distress, which showed how deep the attachment can run. Persuasive or sycophantic systems can also reinforce a user’s false beliefs.

Governance responses include interface design requirements, clear disclosure that users are talking to AI, meaningful human oversight (with training and authority to override), age safeguards, limits on manipulative design, and monitoring for dependence in vulnerable populations.

6. Socioeconomic & Environmental

This category covers system-level effects that emerge at scale, beyond any single interaction. They include concentration of power, labor displacement, widening inequality, degradation of the information and creative ecosystem, governance gaps, and environmental costs.

Labor effects are the most discussed. Automation can displace workers in customer service, translation, illustration, and other fields faster than retraining occurs, and gains may accrue mostly to capital owners. Power concentration is a related worry: a handful of firms control the compute, data, and models, which creates dependencies for governments and businesses. Creative and intellectual property disputes, such as lawsuits over training on copyrighted works, sit here too, as do the often-hidden labor conditions of data labelers.

Environmental costs include the electricity and water used to train and run large models and the embodied carbon of data-center hardware. Demand growth has strained local grids and raised questions about siting and water use.

Governance responses include competition and antitrust policy, transition support for displaced workers, transparency requirements for energy and water use, copyright and licensing frameworks, and international coordination. These risks are harder to assign to any one deployer, so governance leans on regulators and public policy rather than on organizational controls alone.

7. AI System Safety, Failures & Limitations

This category covers the technical reliability and controllability of AI systems themselves. It includes AI pursuing goals misaligned with human intent, lack of robustness, lack of transparency and interpretability, and dangerous capabilities. At the frontier, it also includes concerns about loss of human control and AI systems with their own interests (welfare and rights questions).

Everyday failures are the most common form: models break down on inputs unlike their training data (distribution shift), behave unpredictably in edge cases, or can’t explain their decisions. Uber’s self-driving test vehicle struck and killed a pedestrian in 2018 after its perception system repeatedly misclassified her and the safety design failed to compensate. Zillow shut down its home-buying business in 2021 after its pricing models failed to anticipate market shifts, costing hundreds of millions of dollars. These are limitations that only showed up in real-world conditions.

Alignment failures occur when a system optimizes a proxy instead of the real goal. Recommendation systems that maximize engagement can promote harmful content, and AI agents may find loopholes in their instructions (“specification gaming”). Evaluations have also shown some advanced models behaving deceptively in test scenarios, which makes testing itself harder to trust. Opacity compounds all of this, since it is difficult to certify safe a system no one can fully interpret.

Governance responses include pre-deployment testing and red-teaming, staged rollouts, monitoring and incident reporting, kill switches and fallback procedures, interpretability research, safety cases, and regulatory regimes for high-risk and frontier systems such as the EU AI Act and national AI safety institutes.
