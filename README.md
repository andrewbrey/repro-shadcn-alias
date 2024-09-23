# Repro for shadcn cli alias issue

Steps taken:

- Scaffold two apps `using_tilde` and `using_at_sign` with
  `npx shadcn@latest init`, following prompts to use default values.
- In _components.json_ for each app, remove the aliases for `ui`, `hooks`, and
  `lib` as they aren't needed to demo this, though they may be afflicted by this
  issue also.
- In _components.json_ for each app, change the `utils` alias to a file called
  `lib/custom_utils_path` prefixed with the appropriate prefix character for
  each app, i.e. for the `using_tilde` app, the value is:
  `"utils": "~/lib/custom_utils_path"`.
- In both apps, rename `lib/utils.ts` to `lib/custom_utils_path.ts`
- In the `using_tilde` app, changed the `paths` _tsconfig.json_ value of
  `"@/*": ["./*"]` to `"~/*": ["./*"]` (i.e. just changed `@` to `~`)
- In the `using_tilde` app _components.json_ changed the `components` alias to
  `~/components` (i.e. just changed `@` to `~`)
- Ran `npx shadcn@latest add button` in each app, and observe that in the
  `using_at_sign` app, my `Button` component is scaffolded correctly respecting
  the custom path for `utils` and in the `using_tilde` app, the `Button` is
  using _nearly_ the default value of of `@/lib/utils` _except_ that it did
  replace the `@` with `~` as if it simply read the alias prefix and nothing
  else from the value.
