# **Uptane: Secure software updates for ground vehicles**

### *The first in a series of Uptane Whitepapers*



## **Compromise--Not If, But When**

In 1953, author Isaac Asimov wrote a short story called [*Sally*](https://en.wikipedia.org/wiki/Sally_(short_story)). Set in 2057, it was built on the premise that cars by that time would not only be self-driving, but also sentient. And, typical of mid 20th century science fiction, the story hinted that helpful and appealing technology carried with it a certain amount of risk. 

In the 68 years since Asimov wrote the story cited above, cars may not have become sentient, but they have certainly grown quite smart.  The increasing number of smart programmable units on today’s cars, called electronic control units, or ECUs, has created a large and complex code base which may contain programming errors. Coupled with the fact that today's cars support a multitude of wireless communication interfaces, the threat of a remote exploit has become more relevant. Furthermore, the distributed manner in which software is developed in the automotive industry, which involves numerous suppliers in a complicated supply chain, offers numerous opportunities for hackers to insert malware or to plan other types of attacks.

The consequences of such exploits have already been demonstrated in both simulated [white hat hacks](https://www.wired.com/2016/09/tesla-responds-chinese-hack-major-security-upgrade/) and through [accidental mishaps](https://www.theverge.com/2018/2/14/17013016/fiat-chrysler-ota-update-problem-jeep). Now, the increasing involvement of nation-state actors and large criminal enterprises, bringing with them resources unavailable to the hackers of yesteryear, means such incidents are becoming more common and potentially more dangerous. The [2020 Global Automotive Cybersecurity Report](https://upstream.auto/upstream-security-global-automotive-cybersecurity-report-2020/), released by UpStream Security in December 2020, shows cyber attacks on vehicles increased 99% from 2018 to 2019, and by 700% since 2016. The results of a compromised ECU could include such worst-case scenarios as tampering with the brakes of an entire fleet of police cars, or causing wide-spread engine failures on a highway during rush hour.

It’s time to acknowledge that the science fiction nightmares of the 20th century have become the real-world challenges of the 21st. The development and implementation of reliable, adaptable, and resilient protection for passenger vehicles and light trucks has never been more important. 

This whitepaper describes Uptane (short-hand verbiage combining the words "Update" and "Octane"), the first software update security system for the automotive industry intentionally developed to resist even attacks by nation-state level actors. It is designed in such a manner that the security of software updates does not degrade all at once, but follows a hierarchy in which different levels of access to vehicles or the automaker’s infrastructure must be gained. By building these multiple levels into the security system, even if an attacker compromises servers, bribes operators, or gains access to vehicular networks, he or she is limited in how much danger they can cause. 

## **Addressing the double-sided nature of software updates and why automobiles are so hard to secure** 
Though not directed against automobiles, the recent [SolarWinds](https://en.wikipedia.org/wiki/SolarWinds) hack in which companies, government agencies, and academic institutions suffered significant data breaches after malware was slipped into a software update, is a sobering reminder that though updates are necessary, they also present chances of realized risk.  The [full impact](https://en.wikipedia.org/wiki/2020_United_States_federal_government_data_breach) of the attack, which is known to have affected computer systems within the U.S. Departments of Defense, State, Homeland Security, Treasury, Commerce, and Energy, is still to be tallied.

Yet, ignoring updates is not a viable option either. As Uptane steering committee member Dr. Justin Cappos, an associate professor of computer science and engineering at NYU Tandon School of Engineering, explained in a December 20, 2020 article in [Yahoo Finance](https://au.finance.yahoo.com/news/why-russias-massive-cyberattack-is-especially-insidious-222912267.html?__s=p54njaazgqic1gqfruk3), nation-state actors are gravitating to such attack targets because updating software is something system maintainers are “supposed to be doing.” In a sense, OEMs and suppliers find themselves between the proverbial rock and a hard place. But, even though updates are risky, he cautioned, ”If you don’t apply software updates, you’re absolutely, definitely vulnerable because old software is vulnerable software.”

For automotive OEMs, securing updates is much more complicated than for a conventional server based system, as the ECUs must deal with limited memory, sometimes nonexistent storage, and the lack of a direct connection to the Internet. In addition, updates need to be applied across a distributed system of devices, which are designed and serviced by differerent suppliers.

## **What are the threats and why isn’t secure transport & code signing enough?**
Increasingly, software updates for automotive ECUs are done using ["software-over-the-air”](https://ihsmarkit.com/research-analysis/remote-software-update-future-growth-business.html) (SOTA) strategies, in which revised versions of software programs are sent to vehicles over the Internet. The strategy is growing in popularity because it is a much quicker and more cost-effective delivery mechanism than traditional methods, such as distributing flash drives or requiring customers to bring in their vehicles for servicing. Established OEMs are recognizing the potential of this strategy. In May 2019, GM [announced](https://www.theverge.com/2019/5/21/18633000/gm-ota-software-updates-digital-platform-reuss) that the 2020 Cadillac CT5 sedan will feature an over-the-air software update mechanism, and that such systems will be standard on all GM vehicles in the next four years. Likewise, Ford’s website announced that it would begin equipping most redesigned vehicles in the U.S. with advanced over-the-air update mechanisms beginning in 2020, starting with the all-electric [Mustang Mach-E](https://media.ford.com/content/fordmedia/fna/us/en/news/2020/05/12/new-ford-over-the-air-updates-mustang-mach-e.html). Moreover,[Toyota and Nissan] (https://asia.nikkei.com/Business/Automobiles/Toyota-and-Nissan-to-upgrade-driving-functions-remotely) are also rolling out OTA update features. Toyota will provide OTA support for the driver assistance system in the 2020 Lexus LS, and Nissan for the 2021 Ariya.  

Unfortunately, connecting ECUs to the Internet expose them to a wide range of attacks, some of which, like SolarWinds, introduce malware masquerading as legitimate updates. The consequences of such attacks could be costly indeed, not only in terms of recalls or lost sales, but also, potentially, in loss of life.

The types of attacks an automotive software security system needs to defend against can be organized into four categories, presented here in order of increasing severity.

**Read updates:** The goal here is intellectual property theft, so these attackers aim to read the contents of software updates. This is generally achieved with an eavesdropping attack, where attackers read unencrypted updates sent from the repository, or the server which contains relevant metadata about images, and sometimes the images themselves, to the vehicles.

**Deny updates:** In this group of attacks, the goal is to deny access to updates so vehicles cannot fix software problems, including newly discovered vulnerabilities. These strategies include:
- *Drop-request attack:* blocks network traffic outside or inside the vehicle to prevent an ECU from receiving any updates.
- *Slow retrieval attack:* slows delivery time of updates to ECUs so a known security vulnerability can be exploited before a corrective patch is received.
- *Freeze attack:* continues to send the last known update to an ECU, even if a newer update exists.
- *Partial bundle installation attack:* allows only part of an update to install by dropping traffic to selected ECUs.

**Deny functionality:** This grouping ups the threat ante a bit further by causing vehicles to fail to function in one of the following ways:
- *Rollback attack:* tricks an ECU into installing outdated software with known vulnerabilities.
- *Endless data attack:* causes an ECU to crash by sending it an infinite amount of data until it runs out of storage.
- *Mixed-bundles attack:* shuts down an ECU by causing it to install incompatible versions of software updates that must not be installed at the same time. Attackers can accomplish this by showing different bundles to different ECUs at the same time.
- *Mix-and-match attack:*  Like the mixed-bundles attack described above, this attack also causes ECUs
to use arbitrary combinations of new versions of updates. However, it is a more serious threat, as it proves the attackers have abused repository keys to sign this bundle. Thus, all the updates provided can be completely arbitrary.

**Control:** The last and most severe threat is if an ECU can be forced to install software of the attacker’s choosing, thus ceding control of that unit. This means an attacker can arbitrarily modify the behavior of a vehicle by overwriting the existing software on an ECU with a malicious software program.

Common defenses against these attacks are to utilize a secure transport communication protocol (e.g., HyperText Transport Protocol Security - HTTPS) and code signing (i.e., using a single-key to sign the code). To prevent the aforementioned attacks, a secure over-the-air update system must do more than secure the communication transport protocol and sign the contents of updates. While a cryptographic signature provides some protection against an arbitrary software attack, this strategy alone is not enough to protect against the other attacks enumerated above. In addition, the signing key itself is a single point of failure for the system. If an attacker gets control of this key, they have full control of all updatable ECUs.

Instead, over-the-air updates require a solution that addresses all of the above attacks and is also compromise resilient. In a compromise resilient system, the security of the entire system does not disintegrate if a hacker obtains control of a repository or a signing key.  In addition, compromise resilient systems like Uptane have built-in mechanisms to make it quicker to recover from such an attack.

## **What does Uptane do differently?**
One of the strengths of Uptane is that it never fights against best practices, but rather expands and/or improves the best practices of highly resilient software. As such, it is designed to work *with* existing systems rather than *replacing* them.  It does this by adding a more realistic approach to software update strategies. As stated above, Uptane acknowledges that compromise is not a matter of *if,* but of *when*. Attacks **will** occur and the best defense is a strategy that can isolate and limit exposure. The building blocks for this state rest on four design principles.

- *Separation of trust:* distributing responsibility for the signing of metadata so if one signing key is compromised, it will not affect other parts of the system.
- *Threshold signatures:* requiring that a fixed number of signatures must be gathered to attest to the authenticity of a file before the update can be downloaded.
- *Explicit and implicit revocation of keys:* providing a mechanism for replacing compromised keys so malevolent parties cannot continue signing metadata to authenticate malware, and ensuring that keys are not used forever.
- *Keeping the most vulnerable keys offline:* mandating that certain signing keys be unavailable to online services, thus making them harder to steal or compromise.

These principles are extracted from an established protocol called [The Update Framework](https://theupdateframework.io/) (TUF), a flexible framework and specification that has proven successful in securing software update systems on repositories. However, researchers realized some changes would be needed to adapt these principles for automotive ECUs. The first is the addition of a second repository, which divides labor and responsibility for different aspects of the verification process. The Image repository holds an accurate inventory of all the images currently on all ECUs on all vehicles maintained by an OEM, and the corresponding metadata. This repository uses offline keys to sign all metadata, making it much harder for attackers to substitute compromised images. The Director repository, which instructs vehicles as to what updates should be installed next, uses online keys to sign its metadata, allowing for easier and faster turnarounds. By combining the two repositories, an OEM can provide both customizability and security for the ECUs on their vehicles.

Another modification made to the basic TUF design has to do with the way Uptane verifies updates. In the verification step, the ECU determines if a file is safe to download by checking its accompanying metadata. An ECU can be designed to use one of two different verification strategies, depending on its power - "full verification" and "partial verification". Full verification requires checking that the hashes and sizes of updates in the signed metadata match the hashes and sizes stored on the Image repository, while partial verification requires only a check of the signature on a subset of metadata from the Director repository.

## **Basic Uptane Design** 

<img align="left" src="papers/images/Uptane_process.png" width="500" style="margin: 0px 20px"/>



The diagram above illustrates how the checks and balances of the system works. The connected components on the right hand side of the diagram are on the vehicle, while the components on the left hand-side represent the repositories. The Image Repository can be thought of as an unchanging source of information about images. It is the keeper of every image currently deployed by the OEM, along with the metadata files that prove their authenticity. The Director Repository knows what software should be distributed to each ECU, given the current state of the repository. 

In the first step in the update process, the ECU sends its vehicle version manifest to the Director repository. The manifest contains signed information about existing images. Using this input, the Director chooses which images should be installed next. The metadata and images are then moved to the ECU, which will run a verification process. The diagram shows a Primary ECU connected to a number of Secondary ECUs. The Primary ECU downloads images and metadata from the repositories, then shares them with some number of Secondary ECUs on the same vehicle. ECUs are generally classified in terms of access to storage space, memory, a power supply, and a direct Internet connection. The form of verification that will be run—full or partial—is also based on the resources of the ECU, as well as how security critical it may be. If the verification indicates no issues, the image can be flashed to the ECU, and the vehicle version manifest will be updated.

Full verification provides better protection to those ECUs that have the memory and storage capacity to conduct the procedure. But, because even the weakest ECUs can be afforded some protection by the less resource-intensive partial method, the security of the system as a whole is improved.


## **How Uptane continues to evolve to meet a changing marketplace**
Uptane is very much a living technology. Over the past three years, as it has standardized its technology, a team of academic, government, and automotive industry personnel have identified key principles to guide the continuing development of its specification and its governing standards. Here are the key values these efforts have embraced and how these changes have manifested themselves.

**Agility:** Agility in this sense refers to staying ahead of the curve on emerging trends in the industry and being able to respond accordingly. Uptane benefits here from the cross-section of experts that continue to contribute to both the [Standard](https://uptane.github.io/papers/uptane-standard.1.1.0.html) governing the technology and the [Deployment Best Practices](https://uptane.github.io/papers/uptane-deployment-best-practices-1.1.0.html) which compile real-world variations from OEMs and first-tier suppliers. Also, as an open source project, anyone can review the standard or the best practices and thus the technology benefits from ongoing feedback derived from real-world implementations.

**Ease of adoption:** As mentioned earlier, Uptane never fights against deployment best practices and one component of that commitment  is that the adopter should not need to reconfigure its software update system just to integrate the framework. For this reason, the Uptane community made a decision early on not to specify data binding formats  or other protocols, operations, usage, and formats. Instead, the Standards team evolved a new form for specifying Uptane that separates interoperability functions, such as backwards compatibility, localization, and deployment, from those essential to reliability, security, and functionality. The latter group of features, which constitute the actual standard, make up the baseline layer for instructions, while all the elements required for interoperability are specified in a second layer, known as a Protocols, Operations, Usage, and Formats (POUF) document. By giving implementers the option of creating a [POUF](https://ssl.engineering.nyu.edu/papers/moore_pouf_2020.pdf),  Uptane can be adapted to the needs of existing implementations without requiring extensive modifications. It also makes it easier to add suppliers as needed without having to share proprietary designs.

**Awareness of evolving regulations and standards:** As government and industry address the need for improved automotive security, Uptane is seeking to stay in alignment with emerging regulations and standards governing over-the-air updates and other aspects of cybersecurity. This is achieved by leveraging industry experts with revisions to the Uptane standard and continuing to allow all members in industry provide feedback through the open-source forum.

## **Starting conversations about the challenges ahead**
As vehicles edge ever closer to the designs envisioned in novels and movies in the mid-20th century, there will be new questions to address. Currently, the Uptane group is opening conversation on three such issues. Look for new whitepapers at the end of 2021 and in early 2022 to deal with the following issues.

*Allowing access to ECUs for emergency updates from federal/state/local governments*

As SOTA delivery of updates becomes more sophisticated, government agencies and regulatory bodies, such as the U.S. Department of Transportation or its state or local equivalents, the Department of Homeland Security, or the Federal Emergency Management Agency,  may require automakers to grant them full control over a vehicle in emergency situations. These infrequent but important OTA broadcasts could be due to emergency or disaster routing, scheduled road work, or traffic conditions due to closed exits, failed stoplights, or other conditions.  Accommodating this access will require some re-thinking of how Uptane is configured, particularly in how to prioritize delegations, and perhaps to accommodate a dual Director set-up.

*Security issues related to the use of aftermarket materials*

Aftermarket companies refurbish and reuse equipment following end-of-life support from OEMs. This means introducing ECUs to a vehicle that the OEM has no control over. In addition, because aftermarket suppliers may not have access to the original design, they often reverse engineer the parts to figure out how it works. 
Such an approach perhaps keeps these suppliers from being able to glean all relevant design information about the ECU.

Reflecting the many issues the topic encompasses, this whitepaper will look to effectively frame a number of questions, and whatever answers may be currently available. These questions include:

- How do we deal with aftermarket ECUs that do not have their own Primary? 
- Can these ECUs leverage an OEM's Director repository?
- If an aftermarket ECU does have its own Primary, is it capable of controlling a mutually exclusive set of ECUs?
- Could ownership of the Director be delegated to a third party or owner?

*Uptane's alignment with government and industry regulations*

As previously stated, Uptane continues to monitor regulations and standards. In future whitepapers, Uptane will address how it "dove-tails" with relevant standards, such as:
- Autosar Update Configuration Module (UCM)
- UN R155 & R156
- ISO/SAE 21434
- ISO 24089
- TCG
- IETF SUIT

## **Learn More**
The best place to learn more about Uptane is to go to its [website](https://uptane.github.io/participate.html). Here you can read more on the specification, review the current version of the [Uptane Standard for Design and Implementation](https://uptane.github.io/papers/uptane-standard.1.1.0.html) and the [Deployment Best Practices](https://uptane.github.io/papers/uptane-deployment-best-practices-1.1.0.html) volume, as well as conference presentations, testing information, and other data. We welcome questions, feedback, and suggestions on these materials, the website, or any other aspect of this project. Feel free to raise issues on GitHub or email feedback to jcappos@nyu.edu.

Anyone in the automotive industry, open source community, or security community is welcome to join the Uptane Forum. This is a fairly low volume mailing list  and is used to disseminate large news items, or to plan in-person Uptane workshops. The Uptane standardization initiative is under the direction of the Uptane Standards group and is carried out on a mailing list specifically for this purpose. This mailing list is mainly meant to coordinate the standardization effort. To be added to either list, send an email to lad278@nyu.edu.
