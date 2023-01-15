# Notes to Self

## How to Create a Post
1. Create new posts in `_posts`
2. Name the post YYYY-MM-DD-\<Name>.md
3. At the top of the file, use the following format. Feel free to omit fields that are not needed.

            ---
            title: "Markup: Text Readability Test"
            excerpt: "A bunch of text to test readability."
            categories:
                - Post Formats
            tags:
                - sample post
                - readability
                - test
            ---

4. Upload to repo.

## How to Create Pages
1. Place new pages in `_pages` directory.
2. In the newly created .md file, use the following header.

            ---
            title: <page name>
            layout: collection
            permalink: /<page name>/
            collection: <desired_collection>
            entries_layout: grid
            classes: wide
            ---

3. In `_config.yml` create a new entry under __defaults__ for the newly created page.

            defaults:
                # _<page name>
                - scope:
                    path: ""
                    type: <page name>
                    values:
                    layout: single
                    author_profile: true

4. Update repo.


## How to create Collections
1. Update the `_config.yml` by adding the following under __collections__. __permalink___ will stay the same as shown here.

            collections:
                <collection name>:
                    output: true
                    permalink: /:collection/:path/

2. In `_config.yml` create a new entry under __defaults__ for the newly created page.

            defaults:
                # _<collection name>
                - scope:
                    path: ""
                    type: <collection name>
                    values:
                    layout: single
                    author_profile: false
                    share: true

3. Make a \<name>.md file in the `_pages` folder. Place this at the top of the file. Update the appropriate fields to match the name of the new collection.

            ---
            title: <collection name>
            layout: collection
            permalink: /<collection name>/
            collection: <collection name>
            entries_layout: grid
            classes: wide
            ---

4. Create a new directory `_<collection_name>`.
5. Add content to the newly created `_<collection_name>` directory.
