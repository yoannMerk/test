# test SUPER TEST 1-1-1 last oneasf asdf ]

asdf asdf 
asdf asdf sadf 

# asdf asdf asdf asdf  t est encore assadf sadf 
asdf adsfdf  as adsf



run: |
git tag "Mon_app_$SHA_$DATE"
git push --tags

create_release_note_job:
needs: deploy_job
runs-on: ubuntu-latest
steps:
# Add steps to retrieve commit hashes of the last two tags starting with 'ma-deuxieme-app'
- uses: actions/checkout@v3
- name: Get commit hashes of last two tags for the current app
id: get_commit_hashes
run: |
git fetch --tags
TAGS=$(git tag --sort=-committerdate | grep '^Mon_app_' | head -n 2)
FROM_TAG=$(echo "${TAGS}" | sed -n 2p)
TO_TAG=$(echo "${TAGS}" | sed -n 1p)
echo "FROM_TAG_HASH=$(git rev-parse "${FROM_TAG}")" >> $GITHUB_ENV
echo "TO_TAG_HASH=$(git rev-parse "${TO_TAG}")" >> $GITHUB_ENV
echo "FROM_TAG: ${FROM_TAG}"
echo "TO_TAG: ${TO_TAG}"

      - name: Get commits between tags
        id: get_commits_between_tags
        run: |
          git log --pretty=format:"%h %s" $FROM_TAG_HASH..$TO_TAG_HASH > commits.txt

      - name: Display commits
        run: cat commits.txt