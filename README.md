<details>
  <summary><h3>Bounty board (React & Tailwind)</h3></summary>

  ```tsx
  import { useEffect, useState } from 'react';
  import { algora, type AlgoraOutput } from '@algora/sdk';

  // TODO: Use your own Algora handle
  const org = 'acme';
  const limit = 3;

  type RemoteData<T> =
    | { _tag: 'loading' }
    | { _tag: 'failure'; error: Error }
    | { _tag: 'success'; data: T };

  type Bounty = AlgoraOutput['bounty']['list']['items'][number];

  export function Bounties() {
    const [bounties, setBounties] = useState<RemoteData<Bounty[]>>({ _tag: 'loading' });

    useEffect(() => {
      const ac = new AbortController();

      algora.bounty.list
        .query({ org, limit, status: 'active' }, { signal: ac.signal })
        .then(({ items: data }) => setBounties({ _tag: 'success', data }))
        .catch((error) => setBounties({ _tag: 'failure', error }));

      return () => ac.abort();
    }, []);

    if (bounties._tag === 'loading') return <div>Loading...</div>;
    if (bounties._tag === 'failure') return <div>Error: {bounties.error.message}</div>;

    return (
      <div className="grid gap-4 md:grid-cols-3">
        {bounties.data.map((bounty) => (
          <a
            key={bounty.id}
            href={bounty.url}
            target="_blank"
            rel="noopener noreferrer"
            className="block p-4 border rounded-lg hover:border-indigo-500 transition-colors"
          >
            <div className="font-semibold">{bounty.title}</div>
            <div className="text-sm text-indigo-600">{bounty.reward_formatted}</div>
          </a>
        ))}
      </div>
    );
  }
  ```
</details>