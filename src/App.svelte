<script>
    import {
        Headings,
        Button,
        InputWithLabel,
        CheckboxWithLabel,
        Card,
        Modal,
        ThankModalContent,
        GithubIcon,
    } from './components'
    import {
        subscriptionOptions,
        enterMailWording,
        freetrialWording,
        informationWording,
        privacyWording,
        subscribeWording,
        tryNowWording,
        workMailWording,
        termsAndConditions,
    } from './wordings'
    import { buildOptionGroup, getSelectedValues } from './helpers'

    const { label: termsAndConditionsLabel, name: termsAndConditionsName } =
        termsAndConditions[0]

    let emailValue = ''
    let { termsAndConditions: termsAndConditionsAgreed } =
        buildOptionGroup(termsAndConditions)

    let selectedSubscriptions = buildOptionGroup(subscriptionOptions)

    let isSubmitted = false

    // TODO: Add validation for email onSubmit
    const handleSubmit = (e) => (isSubmitted = true)
    const closeModal = () => (isSubmitted = false)
</script>

<main>
    <div class="icon-container">
        <GithubIcon url="https://github.com/muhsinkamil/svelte-subscription" />
    </div>
    <Card>
        <div class="headings_wrapper">
            <Headings heading={tryNowWording} />
            <h6 class="sub_heading">{freetrialWording}</h6>
        </div>

        <form on:submit|preventDefault={handleSubmit}>
            <InputWithLabel
                label={workMailWording}
                placeholder={enterMailWording}
                name="email"
                type="email"
                bind:value={emailValue}
            />

            <div class="options_container">
                <CheckboxWithLabel
                    label={termsAndConditionsLabel}
                    name={termsAndConditionsName}
                    optionName={termsAndConditionsName}
                    bind:checked={termsAndConditionsAgreed}
                />
                {#each subscriptionOptions as { label, name: optionName } (label)}
                    <CheckboxWithLabel
                        {label}
                        name="subscriptions"
                        {optionName}
                        bind:checked={selectedSubscriptions[optionName]}
                    />
                {/each}
            </div>

            <Button
                label={subscribeWording}
                disabled={!termsAndConditionsAgreed || !emailValue}
            />
        </form>

        <p class="paragraph_privacy">
            {privacyWording}
            <a href="/" class="links">{informationWording}</a>
        </p>
        <Modal
            show={isSubmitted}
            okBtnText="ok"
            on:overlayClick={closeModal}
            on:okClick={closeModal}
        >
            <ThankModalContent
                {emailValue}
                subscriptionList={getSelectedValues(selectedSubscriptions)}
            />
        </Modal>
    </Card>
</main>

<style>
    main {
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: rgb(226, 222, 222);
    }

    .sub_heading {
        font-size: 0.9em;
        font-weight: 200;
    }

    .headings_wrapper {
        padding: 0 0 1rem 0;
    }

    .options_container {
        padding: 5px 0 20px 0;
        display: flex;
        flex-direction: column;
    }

    .paragraph_privacy {
        font-size: 0.9em;
        padding: 20px 0 0 0;
    }

    .links {
        text-decoration: none;
        color: #486ef3;
    }

    .icon-container {
        position: fixed;
        right: 2%;
        top: 3%;
        cursor: pointer;
    }
</style>
