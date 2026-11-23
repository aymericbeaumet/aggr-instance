---
title: GitHub Actions accepts invalid YAML
link: https://yossarian.net/til/post/github-actions-accepts-invalid-yaml/
source: lobste-rs
published: 2026-11-22T21:49:18Z
updated: 2026-11-22T21:49:18Z
first_seen: 2026-11-23T09:43:22.615423405Z
authors:
- yossarian.net by yossarian
labels:
- programming
summary: Comments
content: extracted
html: 2026-11-22-github-actions-accepts-invalid-yaml.html
---

TL;DR: GitHub Actions accepts several invalid YAML productions, making it difficult (read: impossible) to parse some real-world action and workflow definitions with a properly conforming YAML parser.

This is related to [zizmor#1116](https://github.com/zizmorcore/zizmor/issues/1116), which has been bugging me for a year and a half.

Last April, a user reported that [zizmor](https://docs.zizmor.sh) was rejecting a workflow containing a step that looks like this:

```yaml
- name: Create NPM Remote Repository JSON
  uses: jsdaniell/create-json@v1.2.3
  with:
    name: "npm-remote-template.json"
    json: '{
      "key": "npm-remote",
      "rclass": "remote",
      "packageType": "npm",
      "url": "https://registry.npmjs.org"
    }'
```

Apart from the use of a multi-line flow-style string for the `json:` key, nothing particularly stands out about this step.

Except: it isn't valid YAML! YAML says this about flow-style nodes that are nested inside of a block-style parent collection (read: map or array):

> Flow nodes must be indented by at least one more space than the parent block collection. Note that flow nodes may begin on a following line.
>
> *Source*: [YAML 1.2.2 § 8.2.3](https://yaml.org/spec/1.2.2/#823-block-nodes)

That means the *correct* syntax is instead:

```yaml
- name: Create NPM Remote Repository JSON
  uses: jsdaniell/create-json@v1.2.3
  with:
    name: "npm-remote-template.json"
    json: '{
      "key": "npm-remote",
      "rclass": "remote",
      "packageType": "npm",
      "url": "https://registry.npmjs.org"
     }'
```

The difference is barely visible:

```diff
-    }'
+     }'
```

YAML is a big and complicated spec, so it didn't even occur to me that this *was* invalid. Instead, I opened [tree-sitter-yaml#36](https://github.com/tree-sitter-grammars/tree-sitter-yaml/pull/36) to "fix" it. I've now opened [tree-sitter-yaml#51](https://github.com/tree-sitter-grammars/tree-sitter-yaml/pull/51) to revert that misbegotten fix.

On GitHub's side, I'm not sure if they have a clean path towards spec conformance here. Much of the code behind Actions is proprietary, but it seems likely that the underlying parser on their end is [YamlDotNet](https://www.nuget.org/packages/yamldotnet/).

Actions [appears to use YamlDotNet 5.3.0](https://github.com/actions/runner/blob/7e211dc69c347b78fc398e630e74facb11a13235/src/Sdk/Sdk.csproj#L29), which was [released in 2018](https://github.com/aaubry/YamlDotNet/releases/tag/v5.3.0). However, even upgrading to the newest version (18.1.0 as of writing) wouldn't fix this particular bug, since it's [still present](https://github.com/aaubry/YamlDotNet/blob/748334a8fa7c227740018b284b71ad95cc6b7fc7/YamlDotNet/Core/Scanner.cs#L1841-L1844) on that version (note the `!isSingleQuoted` guard).

My best guess is that a 2020 patch ([YamlDotNet#467](https://github.com/aaubry/YamlDotNet/pull/467)) only fixed the double-quoted form because YAML's official test suite only tests rule 8.2.3 with a double-quoted string, despite it applying to both forms.
