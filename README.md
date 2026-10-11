life os

an operating system for my life.

-------------------------------------------------------------------------------

    goal
        -
        overall my goal by the end of the month is create an mvp of an 'os'
        for myself. something that can take information about my day to day
        and create a jarvis like dashboard. that is the overall goal and im
        sure we will deviate but the big picture is a hub for all of the day
        to day things that need to get done but also long term goals, skills
        wanting to learn etc. i have ideas throughout the days and id love to
        be able to centralize them in one place. things like a grocery list
        checker to niche things like a temu promo bot helper. things i can
        build out; projects and passion things i can centralize somewhere.

        this is built for me only. single user. no accounts, no onboarding,
        no multi tenant anything. i can hardcode assumptions and build
        exactly what works for me.

        this is a long project and i want to build it right. beautiful, not
        rushed.

    scope for october
        -
        mvp means one module working end to end, on a foundation the rest can
        plug into.

        this is headless first. no ui in the mvp. the action button is
        already a headless flow, just an authenticated request with text in
        it, so the system can be live and in daily use before a single pixel
        exists.

        in scope:
            the spine. how modules register, where data lives, how outside
            things talk to the system.
            tasks as the first module, proving the spine works.
            capture from the iphone action button.
            the brain, a single ai layer everything routes through.
            a terminal readout script. the fake ui. see below.

        not in scope yet:
            the real dashboard and any visual ui.
            groceries, resource hub, design studio, links.
            telegram or whatsapp bot, though the spine should make it easy
            later.

        the point of october is not how much got built. it is whether module
        two takes a weekend instead of a rebuild.

    the spine
        -
        every module plugs into the same places. a module declares what it
        is, what data it owns, and what it shows on the dashboard. adding a
        module should mean dropping in a folder and registering it, not
        touching the core.

        data lives in one database, one schema, structured so the ai layer
        can actually query across modules. this matters more than it sounds.
        if groceries and tasks store things in totally different shapes, the
        jarvis part never works, because it cannot see across them.

    capture
        -
        every place an idea happens needs a way in.

            iphone action button, the main door.
            the web ui directly.
            later: bots, voice, email forwarding.

        all of them hit the same backend entry point. one path in, not four.

    the brain
        -
        one ai layer that every ai call in the system goes through. text in,
        structured data out.

        starting with the claude api because reliable structured output
        matters more than anything else right now. but nothing else in the
        system knows or cares which provider is behind it, so swapping to a
        local model later for cheap high frequency work is a config change,
        not a rewrite.

        what it does to start:
            parse natural language into a real task, with due date and
            priority.
            prioritize what matters today.
            later: summaries, surfacing stale things, connecting modules.

    the terminal readout
        -
        going headless has one real risk. no visual feedback means a bad
        data model stays hidden until i finally try to render something, and
        by then there is a month of work sitting on top of it.

        so one script, early. it prints tasks as a plain text dashboard in
        the terminal. that is the fake ui. if it reads badly there, the data
        is wrong, and i find that out in week one instead of month three.

        the other risk is that ui at the end quietly becomes ui never, since
        the interesting work is already done by then. so the ui is not gated
        on a date, it is gated on usage. once this has been in daily use
        headless for two weeks and the task flow stops annoying me, the
        dashboard gets built. two weeks of using my own system tells me what
        actually belongs on it. building the ui first is just guessing.

    ui
        -
        this comes after the headless mvp, not during it.

        for the graphical user interface of this project, i want it to be
        compiled of components that i comprise it of within the files. i want
        it to have a pleasing feel both mobile and web version. remember this
        is an operating system that is meant for daily life. the action
        button means mobile is the main entry point most days, so mobile is
        not an afterthought.

        components live in a structured file hierarchy. i pull code and
        components i like from around the web, clean them up, and drop them
        into that structure. each one is filed so it can be found by keyword
        and pulled in when it is mentioned, instead of being rebuilt from
        scratch every time.

        shared design tokens from the start. color, type, spacing, radius,
        shadow. imported components get normalized to those tokens so
        everything feels like one thing instead of a scrapbook.

    security
        -
        this holds my life data and it is exposed to the internet, so
        security gets handled per module as it is built, not as one audit at
        the end. finding problems after building on top of them is the
        expensive way.

        the constants:
            every request to the backend is authenticated, so an endpoint
            someone finds is useless to them.
            secrets and api keys never touch the repo.
            the database is locked down and never directly reachable from
            the client.

    build order
        -
        headless:
            1. spine, data layer, auth
            2. tasks module
            3. terminal readout script
            4. action button capture
            5. the brain wired into tasks
            6. live in it. daily use, two weeks.

        then:
            7. module two, which is the real test of whether any of this
            worked
            8. dashboard shell and ui, informed by what two weeks of real
            use actually proved i needed
