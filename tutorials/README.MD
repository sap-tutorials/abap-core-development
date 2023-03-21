# Finished Tutorials
This folder is for finished tutorials that you want to publish to the QA system to check how they look "in the real world". When you are first starting, use the `work-in-progress` folder.

>There is little difference between the `work-in-progress` and `tutorials` folders, except:
  - A set of tests are run on the tutorials in this folder. If your tutorial is not ready, you will "break the build" and administrators will be alerted.
  - Only tutorials in this folder are published to the QA server.

So, when putting tutorials in this folder, make sure they are ready.

## Checks before going live
Before a tutorial can go live, the following checks have to performed:

### General
- [ ] All product names correct (e.g., `SAP Web IDE`, not `WEBIDE`, `Web IDE`, `the SAP Web IDE`) and all names of things that are not products lower-case (e.g., `SAP HANA dynamic tiering`, not `SAP HANA Dynamic Tiering`).
- [ ] Page URL matches SEO rules and 1DX naming conventions (i.e. no underscores, special characters, no fill words, and abbreviations).
- [ ] No broken links (images or URLs).
- [ ] All images have alt text.
- [ ] Relative links for system-internal pages, and absolute links only for external.
- [ ] No links to `*.exe` or other suspicious files, or to Microsoft Word or PowerPoint files.

### Styleguide
- [ ] Follows styleguide (i.e., all metadata entered, and all sections filled in)

### Content
- [ ] No typos and misspellings

---

For guidance on creating tutorials, please see the tutorials [Create developer tutorials](https://www-qa.sap.com/developer/groups/docs-tutorial.html).
