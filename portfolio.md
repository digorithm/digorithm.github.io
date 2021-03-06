---
layout: page 
title: Projects 
permalink: /projects/
---

---
#### EpiMobile: Pathogen Point of Care Diagnosis and Global Surveillance using Mobile Devices ([Paper](/content/epimobile_paper.pdf), [Code](http://www.github.com/digorithm/epimobile), [Talk](/content/epimobile.pdf))

*Areas: Bioinformatics, Distributed Systems, Healthcare Information Systems*

<div class="abstract">
<p>
Rapid identification of disease causing infectious agents (pathogens) in patient samples is important to mount effective responses to potential, or ongoing, disease outbreaks. New advances in genomic sequencing technologies are enabling more effective point-of-care diagnosis – where patient samples can be analyzed for the detection of pathogen DNA. However, it has yet to be understood how these technologies can be applied in the field and under resource constraints that hinder cloud access for computation.
</p>

<p>
Here we present EpiMobile, a conceptual model and minimal viable product (MVP) implementation of a genomics point-of-care workflow using mobile devices. EpiMobile enables analyses of genomic data harvested by a portable genome sequencer and the distribution of analysis results to local clinical or healthcare teams as well as national, or global, public health agencies, whilst considering computational processing and Internet connectivity resource constraints.
</p>

<p>
Our evaluation of EpiMobile indicates that it has a minimal resource consumption footprint and is accurate when run of a dataset with known outcomes. However, we emphasize that the conceptual and exploratory nature of our work affects to what extent our results would map to real world settings. We discuss the utility of EpiMobile through a set of usage scenarios currently supported by our MVP implementation. We believe that our work provides an interesting overview of an exciting and emerging healthcare application context as well as proposing an interesting implementation of genomics point-of-care.
</p>
</div>

#### Cross-Compatibility Checker ([Paper](/content/xcc_paper.pdf), [Code](http://www.github.com/digorithm/xcompatibility-checker), [Talk](/content/xcc.pdf))

*Areas: Software Engineering, Web Development*

<div class="abstract">
<p>
Web browsers are built by different organizations and writing software that runs smoothly on all existing browsers is a challenging task. Due to the pace that browsers implement or adopt certain web features, users' experience may be hindered by visual and functional incompatibilities due to unsupported or non-standard features in a given browser.
</p>

<p>
In order to address the detection of cross-browser incompatibilities in early stages of web development, we propose XCompatibility-Checker a lightweight tool that automates the identification of features that are not supported by different browsers. The tool’s evaluation was twofold (i) it was able to detect cross-browser incompatibilities in 38 open source web applications; as well as (ii) a user study and qualitative survey indicates that the tool improves developers' awareness and ability to detect cross-browser incompatibilities.
</p>
<p>
Therefore, our proposed tool helps web developers improve the quality of their web applications.
</p>
</div>

#### Salvador: A Decentralized, Secure Backup System ([Paper](/content/salvador_paper.pdf), [Talk](http://slides.com/rodrigoaraujo-1/salvador))

*Areas: Distributed Systems*

<div class="abstract">
<p>
Many modern systems that offer file backup and sharing rely on distributed storage in order to achieve load balancing, fault-tolerance, and scalability. Systems like Dropbox provide a form of interaction that resembles a client-server from the user’s perspective while leveraging a Cloud-based architecture and that is highly distributed. Dropbox is a file hosting service that enables users to store and share personal data with semantics similar to those of a local file system while storing the data across many geographically distributed servers.
</p>

<p>
Other systems adopt a P2P architecture, such as pStore and the Cooperative File System (CFS). pStore is a secure, distributed backup system that leverages unused disk space that offers features such as encryption and sharing and CFS is a read-only storage system. In this work we present Salvador, a system that draws inspiration from many of the systems mentioned.
</p>

<p>
Salvador does not offer file sharing capabilities, which means that the only user with access to the entire file and allowed to backup or restore that file is its owner and creator of that file. At the same time, the semantics of Salvador are similar to that of Dropbox in the sense that users can easily create, edit, or delete files on their computer while being able to recover such files by collecting its parts from peers and reconstructing the file locally.
</p>
</div>

#### TDWiki - The Technical Debt Wiki ([Paper](/content/tdwiki_paper.pdf))

*Areas: Software Engineering, Technical Debt*

<div class="abstract">
<p>
TDWiki is a collaborative computational infrastructure for supporting technical debt knowledge sharing and evolution. It was built to be an autonomous system to extract data about researches being made on Technical Debt field and provide Intelligence over this data and visualizations of this information, also it has a social/collaborative aspect, where Researchers/Engineers/Managers can help to grow the knowledge
</p>
</div>
