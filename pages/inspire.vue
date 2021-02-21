<template>
  <section class="section">
    <h2 class="title">What is 10Bagger.com?</h2>
    <p>
      10Baggers.com parses through the option chain to identitfy the options
      that would take the least amount of movement from the underlying stock to
      make the option play a 1000% or 10x or 10 Bagger play.
    </p>
    <br />
    <h2 class="title">Cool, how do I use this?</h2>
    <ol>
      <li>Search for the ticker of the stock you are interested in trading</li>
      <li>Select an Expiration Date from the dropdown</li>
      <li>
        By default you'll see which option requires the least amount of price
        movement to become a 10 bagger at the top!
      </li>
      <li>
        You can sort by Target price and see if any 10baggers match your TA
        target prices
      </li>
      <li>
        You can sort by Strike price to see what it would take for the ATM call
        to 10x in value
      </li>
      <li>
        Additionally, if you'd like to see the most trades options in that
        change, sort by Open Interest
      </li>
    </ol>
    <br />
    <h2 class="title">
      How this the required percent change being calculated?
    </h2>
    <p>
      We look at the initial cost of the options and calculate how far into the
      money the option needs to expire in order to generate enough instrinsic
      value to 10x the cost of the option. Its important to remember that these
      calculation assumes holding till expiration but some cases you can get the
      10 bagger prior to expiration of the option.
    </p>
    <br />
    <h2 class="title">
      Awesome, I made my first 10 bagger, how do I support? (No one actually
      says this)
    </h2>
    <p>
      We'll keep working on this project as along as it keeps helping people
      make money but the cost of business is always a barrier that can only be
      broken with profitablity
    </p>
    <form action="https://www.paypal.com/donate" method="post" target="_top">
      <input type="hidden" name="business" value="5XKGKHHC3VDAJ" />
      <input type="hidden" name="item_name" value="10Bagger Donation" />
      <input type="hidden" name="currency_code" value="USD" />
      <input
        type="image"
        src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif"
        border="0"
        name="submit"
        title="PayPal - The safer, easier way to pay online!"
        alt="Donate with PayPal button"
      />
      <img
        alt=""
        border="0"
        src="https://www.paypal.com/en_US/i/scr/pixel.gif"
        width="1"
        height="1"
      />
    </form>
  </section>
</template>
