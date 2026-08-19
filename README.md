# TAD3 Protocol

**Primary source:** [U.S. Securities and Exchange Commission, *Transfer Agent Regulations*, Exchange Act Release No. 34-76743, File No. S7-27-15 (Dec. 22, 2015), pp. 21–23](https://www.sec.gov/files/rules/concept/2015/34-76743.pdf#page=22).

## The TAD concept

TAD3 starts with an older market-structure proposal, not with blockchain. In its 2015 history of U.S. clearance and settlement, the SEC returned to a 1969 study commissioned by the American Stock Exchange from North American Rockwell Information Systems Company. The SEC identifies the source as *Securities Industry Overview, Final Report to the American Stock Exchange* (1969), which it calls the **Rockwell Study**.[^rockwell]

The SEC explains that Rockwell proposed “a decentralized network of individual transfer agent depositories.” Under that design, depositing securities with the issuer's transfer agent would make “each transfer agent an independent depository for its respective issuers.”[^tad] The transfer agent would maintain the issuer's shareholder register electronically, and settlement would be reflected by debiting and crediting the parties' positions on that issuer register rather than by moving certificates between market participants.[^mechanics]

That is the historical **Transfer Agent Depository (TAD)** concept from which TAD3 takes its name and architectural starting point.

### Direct connection to the Rockwell Study

The SEC provides an unusually specific source trail back to Rockwell's original report:

- SEC footnote 19 identifies the report as **North American Rockwell Information Systems Company, *Securities Industry Overview, Final Report to the American Stock Exchange* (1969)**.[^rockwell]
- SEC footnote 52 cites Rockwell pages **3, 9, 14, 31, 39, 43, 77, and 98** for the TAD proposal.[^tad]
- SEC footnote 53 cites Rockwell pages **39–43** for the proposed electronic-register and settlement mechanics.[^mechanics]
- The SEC separately cites Rockwell page **101** when discussing the possibility of combining transfer-agent and registrar functions while preserving the registrar's audit and shareholder-protection role.[^registrar]

The SEC describes the proposal as theoretical in 1969. Its defining structure, however, is concrete: a national clearing layer connecting a **decentralized network of issuer-level depositories**, with the transfer agent maintaining the issuer's authoritative register and applying settlement directly to that register.[^tad]

## From TAD to TAD3

TAD3 treats that 1969 architecture as the starting point for a modern protocol. Issuer-side transfer agents remain the authoritative recordkeepers for the securities they service, while shared digital infrastructure supplies interoperability among agents, issuers, investors, and trading systems. The **3** denotes the adaptation of the TAD model to Web3 infrastructure; it is not a claim that Rockwell's 1969 proposal contemplated distributed ledgers.

This distinction is important. TAD3 is not intended to reproduce a single central securities depository on a blockchain. Its organizing model is the alternative path documented by the SEC from the Rockwell Study: independent transfer-agent depositories connected through common settlement infrastructure, with ownership changes capable of reaching the issuer register itself.

## Protocol development

For the moment, I'd like to keep the TAD concepts within this GitHub organization since I think it will work well to have us iteratively improve the platform while we're still the only agent using it. Accordingly, we need to flesh out much of the design spec, which is only preliminarily implemented in the Python interface. This relatively easily gets into formalizing the interface requirements and standards through tracks used on other Web3 systems for community consensus and documentation in the source repo.

To be migrated from the `website` (team) policies.

## References

[^rockwell]: SEC, *Transfer Agent Regulations*, Release No. 34-76743, p. 14 n.19 and pp. 21–23, available in the [Commission's PDF](https://www.sec.gov/files/rules/concept/2015/34-76743.pdf) and the [Federal Register text](https://www.federalregister.gov/documents/2015/12/31/2015-32755/transfer-agent-regulations#p-95).
[^tad]: Id. at pp. 21–22 & n.52 (citing Rockwell Study at 3, 9, 14, 31, 39, 43, 77, 98).
[^mechanics]: Id. at pp. 22–23 & n.53 (citing Rockwell Study at 39–43).
[^registrar]: Id. at pp. 21–22 & nn.50–51 (citing Rockwell Study at 101).
