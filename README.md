return (
      <div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
        {bounties._tag === 'loading' && <p>Loading bounties...</p>}
        {bounties._tag === 'failure' && <p>Error: {bounties.error.message}</p>}
        {bounties._tag === 'success' &&
          bounties.data.map((bounty) => (
            <Link
              key={bounty.id}
              href={bounty.url}
              target="_blank"
              rel="noopener noreferrer"
              className="block p-4 border rounded-lg hover:border-accent-500 transition-colors"
            >
              <div className="font-bold text-gray-900">{bounty.title}</div>
              <div className="mt-1 text-sm text-gray-500">{bounty.reward_formatted}</div>
            </Link>
          ))}
      </div>
    );
  }