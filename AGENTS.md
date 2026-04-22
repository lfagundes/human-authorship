# AGENTS.md

This file provides guidance for AI coding assistants when modifying any content in this repository.

## Human Written

The header of this file is human written and cannot be changed by AI assistant.

The token END-HUMAN-WRITTEN below marks the end of the header, and so agents may modify from that part below.

## Commit session logging

Every instruction input to AI assistant is considered source code and should be preserved.

There is a directory '_commit_log', it always contains full log of all instructions given to AI that generates a commit, with a brief of what the AI assistant did on each instruction.

When starting a new session, assistant MUST:
- Execute a date command to extract session timestamp in format %Y%m%d-%H%M%S

Before changing any file in the repository, assistant MUST:
- Do a git status on _commit_log
  - If it's clean, do a git rm of everything inside
- Create file _commit_log/session-TIMESTAMP.md
- All interactions of that section must be logged
  - Each user instruction must be a section "# Instruction $N", where N is an incremental number starting at 1
  - The instruction content, as typed by user, goes inside this section
  - AI response is structured in subsections inside this section
  - If user instruction includes reading a prompt to be executed
    - do a git status on that file
    - if it's not in repository, or is modified, copy that prompt version into _commit_log directory
    - include a subsection of response telling where the file was copied to
  - A git add on the file must be done after the file is written
  - For every new instruction, a new section incrementing N must be created
  - Make sure the instruction to make a commit is also logged before the commit is made

The only exception is a session that is a merge commit, that is merging a branch. In this case:
- Only do the merge if this is a fresh session, otherwise, request user to start fresh
- Study the branch and make a relevant commit message of the whole branch
- Use --no-ff option to merge the branch

## Commit messages

Any commit message by an AI assistant MUST contain information about the LLM model used in the commit with the following template:

AI Agent: [LLM model name] by [Company]

## Constraints

- You are not allowed to look into sibling or parent directories
- Never commit without explicit instructions from user

END-HUMAN-WRITTEN
