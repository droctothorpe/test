```mermaid
%%{init: {'themeVariables': {'fontSize': '12px'}}}%%
flowchart TD


subgraph releaseType["Release Type"]
  major
  minor
  patch
end

%% entrypoints
major --> cutmajor["`cut release-major.0.0 branch from master branch in upstream`"]
minor --> cutminor["cut release-major.mino branch from master branch in upstream"]
patch --> cutpatch["cut release-major.minor.patch branch from release-MAJOR-MINOR branch"]

%% cherry-pick patches
cutpatch --> cherrypick["cherrypick commits from master to release-major.minor.patch"]
cherrypick --> prcherrypicks["pr from release-major.minor.patch to release-major.minor"]
prcherrypicks --> mergecherrypicks["merge pr into release-major.minor"]

%% converge on version updates
cutmajor --> updateversions["update multiple version tags in source"]
cutminor --> updateversions
mergecherrypicks --> updateversions
updateversions --> prversions["push the version tag updates to your fork and PR the upstream release branch"]
prversions --> mergeversions["merge pr into release branch"]

mergeversions --> publishimages["run image publication workflow (if applicable)"]
mergeversions --> publishsdks["run sdk publication workflow\n(if applicable)"]
