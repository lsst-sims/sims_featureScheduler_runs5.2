Run some simulations to try and cover the DESI footprint


desi1:  Add a tier to get the desi observations
desit2: Same, but push the start of rolling back a year



Initial thoughts:  Let's try making some blob-like surveys with the DESI footprint, and we'll make a footprint basis function that actually uses number of observations rather than relative I guess. Kill the survey objects after year 4, set to only go if moon is down, moon not rising for x time, and twilight not starting for x time. 


after first pass--looks pretty good. There's a small strip that didn't complete by Y4 because there's an area limit set. So maybe lift the area requirement (or increase HA range) after Y3 to make sure it fills.

-----



The request (with the details about the request) comes from a proposal for an MOU-level kind of agreement between Rubin and DESI, so it doesn't itself contain justification for the science. The question really is about meeting their request and what it would cost -- they are offering a time trade with DESI. 

That said, from previous DESI related questions, it's about getting better depth so they can find good targets for their spectrograph and from seeing the people involved (David Schlegel) are already familiar with opsim outputs, I would guess the number of visits has been estimated from estimates of depth from typical depth Rubin achieves after 40 visits in each of these bands at this declination range. I'm not sure why it's couched as minimum number of visits. 
The 30 visits in u and g options come from the idea of only being able to get part-way to their request (because they first only asked for 40). If we really cannot get to 30 and 40 for technical reasons, then intermediate "this is as good as we could do" simulations would be good to also evaluate. Getting to 30 and 40 by decimating the rest of the survey is ok, if that's the only way to do it -- at this stage, that is part of the question for the SCOC (are they willing to make this trade?) but we should be able to justify why that is the only way to be able to meet their request.  The tricky part is getting to the requested number of visits per band while preserving the rest of the survey as much as possible.

I imagine 29 is about the same as 30, but 25 is not. The means may have to be a bit bigger than 30/40 now that I think about it, as they said 
"We especially request that the first two years of Rubin ensure a minimum of 40 visits in each of u,g,r,i,z in the SGC rolling cadence footprint at -15 < dec < 4 and in the NGC at -9 < dec < 5 deg, and the same number of visits in the remainder of the footprints after four years." (the rolling cadence part of the footprint is the sgc_y2 and ngc_y2 regions in my snippet above). 

I know investigating various ways to get to the required number of visits may take a while -- how long do you think for a first pass? 

Lynne

On Wed, Feb 18, 2026 at 10:26 AM Peter Yoachim <yoachim@uw.edu> wrote:

    There's nothing particularly difficult about simulating the DESI
    coverage, just some decisions to make about how to implement it. It
    would be good for someone to think of some science drivers behind it,
    especially since defining in terms of number of visits rather than depth
    can mean we meet what was asked for but not really have great results.
    And because we'll probably want to see what happens if we only get
    part-way to their request.

    P


    On 2/18/26 2:32 AM, Lynne Jones wrote:
    > Hi Peter, Sending this in email because it might be easier to keep
    > track of the information there. But here's the deal -- DESI have a
    > proposal for Rubin and we need to help evaluate the impact, so I'd
    > like you to try out some simulations. 
    > ZjQcmQRYFpfptBannerStart
    > This Message Is From an Untrusted Sender
    > You have not previously corresponded with this sender.
    > See https://itconnect.uw.edu/email-tags for additional information.
    > Please contact the UW-IT Service Center, help@uw.edu 206.221.5000, for
    > assistance.
    > ZjQcmQRYFpfptBannerEnd
    > Hi Peter,
    >
    > Sending this in email because it might be easier to keep track of the
    > information there.
    >
    > But here's the deal -- DESI have a proposal for Rubin and we need to
    > help evaluate the impact, so I'd like you to try out some simulations.
    >
    > DESI want 40 visits in each of ugriz over a region in the northern
    > part of the WFD, one part of the region completed by the end of Y2 and
    > the other (smaller) part completed by the end of Y4. The Y2 region
    > matches the "first active high-cadence" part of the WFD rolling.
    >
    > The regions are defined like this:
    >
    > nside = 64
    > s = maf.HealpixSlicer(nside=nside)
    > desi_sgc = np.where((s.slice_points['dec'] <= np.radians(4))
    >                     & (s.slice_points['dec'] >= np.radians(-20))
    >                     & (((s.slice_points['galb'] < np.radians(-25)) &
    > (s.slice_points['ra'] > np.radians(270)))
    >                     | ((s.slice_points['galb'] < np.radians(-45)) &
    > (s.slice_points['ra'] < np.radians(90)))),
    >                     1, 0)
    > desi_sgc_y2 = np.where((s.slice_points['dec'] >= np.radians(-15)) &
    > (s.slice_points['dec'] <= np.radians(4)),
    >                        desi_sgc, 0)
    > desi_ngc = np.where((s.slice_points['dec'] <= np.radians(15))
    >                     & (s.slice_points['dec'] >= np.radians(-9))
    >                     & (((s.slice_points['galb'] > np.radians(20)) &
    > (s.slice_points['ra'] < np.radians(180)))
    >                        | ((s.slice_points['galb'] > np.radians(28)) &
    > (s.slice_points['ra'] > np.radians(180)) & (s.slice_points['dec'] > 0))
    >                        | ((s.slice_points['galb'] > np.radians(44)) &
    > (s.slice_points['ra'] > np.radians(180)) & (s.slice_points['dec'] < 0))),
    >                     1, 0)
    > desi_ngc_y2 = np.where((s.slice_points['dec'] <= np.radians(5)) &
    > (s.slice_points['dec'] >= np.radians(-9)),
    >                        desi_ngc, 0)
    >
    > desi_both = desi_sgc + desi_ngc
    > desi_y2 = desi_sgc_y2 + desi_ngc_y2
    >
    > alpha = np.where(desi_both > 0, 1, alpha)
    > hp_moll(desi_both + desi_y2, alpha=alpha, rot=(180, 0, 0))
    > hp_moll(desi_both + desi_y2, alpha=alpha, rot=(0, 0, 0))
    >
    >
    >
    >
    > I think this is actually pretty hard. The coverage in v5.1 even by Y4
    > is only about 15 in u band and 23 or so in g band and it's much worse
    > at the end of Y2. The riz bands do ok, coming in at about 40 visits by
    > the end of Y2 already.
    >
    >
    > However... Zeljko and Fed have asked for a set of simulations with:
    >
    > 30 visits in each of u and g, and 40 visits in riz - in the same Y2/Y4
    > areas.
    > 40 visits in each of u and g, and 40 visits in riz -- in the same
    > Y2/Y4 areas as above.
    >
    > Then as well as that, versions of these same simulations where rolling
    > cadence doesn't start until Y3 (instead of starting at Y2).
    >
    > So -- moving enough visits into this area to make these targets is
    > likely tough. My back of the envelope estimate was that given their
    > area is about 25% of the total WFD area, and they're asking for about
    > 50% more visits in that area than we would otherwise get ..
    > In theory we could move u and g visits from other declinations up to the DESI target area, which would be the most straightforward answer.  My guess is that this would mean moving almost all u and g band visits from the rest of the sky, after acquiring enough for templates. Of course, the other question is what about taking time from other bands in the (mostly) the first 2 years (and  somewhat out to year 4) and moving it into u/g, and then also directing most of it into this area.
    > Presumably then the rest of the survey would skew red, in order to come back to the final filter balance desired for the coadds, etc.
    > What do you think?  Do you think this is even possible to simulate?
    > Lynne
    >